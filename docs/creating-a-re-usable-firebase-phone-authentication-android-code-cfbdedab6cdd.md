# 创建一个可重用的 Firebase 手机认证 Android 代码

> 原文：<https://blog.devgenius.io/creating-a-re-usable-firebase-phone-authentication-android-code-cfbdedab6cdd?source=collection_archive---------11----------------------->

![](img/7ffc156a262df027881f7cbf5f12126b.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=99223) 的 [OpenIcons](https://pixabay.com/users/OpenIcons-28911/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=99223)

我们不得不在不同的项目中一遍又一遍地使用一些代码，这是很常见的，将这些代码分成它们自己的类总是很好的，这样它们可以被重用，也可以与其他可重用的代码一起放在一个库模块中。

我使用 LiveData 创建了一个可重用的 Firebase phone 身份验证类来与其他组件通信。我将使用一个 ViewModel，它将为需要监听这些事件的活动/片段提供 Firebase 事件。让我们看看代码。

```
class FirebasePhoneAuth {
    private val firebaseAuth = FirebaseAuth.getInstance()val verificationIdLiveEvent = SingleLiveEvent<String>()
    val phoneAuthSuccessLiveEvent = SingleLiveEvent<FirebaseUser>()
    val phoneAuthFailureLiveEvent = SingleLiveEvent<String>()

    private val callbacks = object : PhoneAuthProvider.OnVerificationStateChangedCallbacks() {

        override fun onVerificationCompleted(credential: PhoneAuthCredential) {
            // This callback will be invoked in two situations:
            // 1 - Instant verification. In some cases the phone number can be instantly
            //     verified without needing to send or enter a verification code.
            // 2 - Auto-retrieval. On some devices Google Play services can automatically
            //     detect the incoming verification SMS and perform verification without
            //     user action.
            signInWithPhoneAuthCredential(credential)
        }

        override fun onVerificationFailed(e: FirebaseException) {
            // This callback is invoked in an invalid request for verification is made,
            // for instance if the the phone number format is not valid.
            phoneAuthFailureLiveEvent.*value* = e.message ?: "FirebaseException"

            if(e is FirebaseAuthInvalidCredentialsException) {
                phoneAuthFailureLiveEvent.*value* = e.message ?: "Invalid request"
            }
            else if(e is FirebaseTooManyRequestsException) {
                phoneAuthFailureLiveEvent.*value* = e.message ?: "The SMS quota for the project has been exceeded"
            }
        }

        override fun onCodeSent(
            verificationId: String,
            token: PhoneAuthProvider.ForceResendingToken
        ) {
            // The SMS verification code has been sent to the provided phone number, we
            // now need to ask the user to enter the code and then construct a credential
            // by combining the code with a verification ID.
            verificationIdLiveEvent.*value* = verificationId
        }
    }

    fun verifyPhoneNumber(
        prefix: String,
        phone: String
    ) {
        PhoneAuthProvider.getInstance().verifyPhoneNumber(
            prefix + phone,
            *PHONE_TIMEOUT*, // Timeout duration
            TimeUnit.SECONDS, // Unit of timeout
            TaskExecutors.*MAIN_THREAD*,
            callbacks
        )
    }

    fun signInWithPhoneAuthCredential(credential: PhoneAuthCredential) {
        firebaseAuth.signInWithCredential(credential)
            .addOnCompleteListener **{** if(**it**.*isSuccessful*) {
                    // Sign in success, update UI with the signed-in user's information
                    val user = **it**.*result*?.*user* phoneAuthSuccessLiveEvent.*value* = user
                }
                else {
                    // Sign in failed, display a message and update the UI
                    if(**it**.*exception* is FirebaseAuthInvalidCredentialsException) {
                        // The verification code entered was invalid
                        phoneAuthFailureLiveEvent.*value* = **it**.*exception*?.message ?: "FirebaseAuthInvalidCredentialsException"
                    }
                    else {
                        phoneAuthFailureLiveEvent.*value* = **it**.*exception*?.message ?: "Task failed"
                    }
                }
            **}** }
}
```

[singliveevent](https://github.com/android/architecture-samples/blob/dev-todo-mvvm-live/todoapp/app/src/main/java/com/example/android/architecture/blueprints/todoapp/SingleLiveEvent.java)是一个生命周期感知的可观察对象，在订阅后只发送新的更新。你可以从谷歌在 Github 的 Android 样本中得到。

让我们检查视图模型代码

```
class AuthViewModel : ViewModel() {
    val firebasePhoneAuthUtils = FirebasePhoneAuth()
    // phone auth init event observable
    val phoneAuthLiveEvent = SingleLiveEvent<Unit>()
    // code entered event observable
    val codeEnteredAuthLiveEvent = SingleLiveEvent<Unit>()
    val codeObservableField = ObservableField("")
}
```

在活动端，我从 FirebasePhoneAuth 观察事件。下面是代码。

```
class AuthActivity : AppCompatActivity() {
    private lateinit var authViewModel: AuthViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val activityAuthBinding: ActivityAuthBinding = DataBindingUtil.setContentView(
            this,
            R.layout.*activity_auth* )
        authViewModel = ViewModelProvider(
            this
        ).get(AuthViewModel::class.*java*)
        observeObservables()
    }private fun observeObservables() {
    // observe required observables here

    authViewModel.phoneAuthLiveEvent.observe(
        this,
        *Observer* **{** // this will execute when user enters number, call verifyPhoneNumber now
            authViewModel.firebasePhoneAuthUtils.verifyPhoneNumber(
                getString(R.string.*phone_prefix*), // +880
                authViewModel.loginObservable.phoneLoginObservable // phone number
            )
        **}** )
    authViewModel.codeEnteredAuthLiveEvent.observe(
        this,
        *Observer* **{** // this will execute when user enters sms code, call signInWithPhoneAuthCredential now
            authViewModel.firebasePhoneAuthUtils.signInWithPhoneAuthCredential(
                PhoneAuthProvider.getCredential(
                    authViewModel.firebasePhoneAuthUtils.verificationIdLiveEvent.*value*!!,
                    authViewModel.codeObservableField.get()!!
                )
            )
        **}** )
    authViewModel.firebasePhoneAuthUtils.phoneAuthSuccessLiveEvent.observe(
        this,
        *Observer* **{** firebaseUser **->** authViewModel.postLogin(firebaseUser?.*phoneNumber*!!).observe(
                this,
                *Observer* **{** // phone auth succeed
                **}** )
        **}** )
    authViewModel.firebasePhoneAuthUtils.phoneAuthFailureLiveEvent.observe(
        this,
        *Observer* **{** // phone auth failed
        **}** )
}}
```

这里，当用户输入电话号码时，phoneAuthLiveEvent 被触发。当用户输入代码时，将触发 codeEnteredAuthLiveEvent。然后用 PhoneAuthProvider 调用 signInWithPhoneAuthCredential。使用用户输入的验证 Id 和代码创建的 PhoneAuthCredentials 对象。最后，当电话认证成功时触发 firebasephone authutils . phoneauthsuccesslivedata，否则当电话认证失败时触发 firebasephone authutils . phoneauthfailurelivedata。

今天到此为止。希望这对你有所帮助。感谢您的阅读。