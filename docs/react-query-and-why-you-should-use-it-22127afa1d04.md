# React 查询以及为什么应该使用它！

> 原文：<https://blog.devgenius.io/react-query-and-why-you-should-use-it-22127afa1d04?source=collection_archive---------2----------------------->

## React Query 简化了获取、缓存和更新数据，即使没有全局状态！

![](img/a47068c008c79f405a71c22864c76ed7.png)

这只棒极了的狗正在吸引人！

当我们创建新的应用程序时，我们有许多架构选择要做。其中之一是创建一个从我们的服务器获取数据的模式。在获取数据时，有几个问题需要考虑；

*   应该使用全局状态缓存获取的数据，还是在每次装载组件时重新获取？
*   如果数据缓存在全局状态中，我们如何知道数据何时过期，或者我们如何将它标记为过期？
*   怎样才能轻易的让缓存失效？
*   我们应该如何处理获取状态，加载，出错等。？
*   当某些查询操作失败时，如何启用重试？

以上所有问题都是使用 React Query 的原因。它简化了为数据获取创建可维护模式的工作！

*在深入举例之前，我需要澄清 React 查询是* ***而不是*** *与* *相同的 Axios 或 fetch API，也不是 Redux 或 Context API 替换。*

让我们来看一个例子。在下面的代码中，我们假设我们有一个 TypeScript 文件，该文件包含一个获取方法来获取返回承诺的用户配置文件。(您可以使用 Axios、fetch-API 或任何其他您喜欢的 HTTP 客户端，只要它返回一个承诺)。下面的示例是显示用户名的个人资料页面。

```
import { useQuery } from 'react-query';
import { getUserProfile } from '@api/profile.ts';export const MyProfile = (): JSX.Element => {
const { data: userProfile } = **useQuery**('UserProfile', getUserProfile);return {
   <h1>Hello, { userProfile.name }</h1>
}}
```

如您所见，我们使用了 useQuery-hook 形式的 React 查询，它有两个参数:缓存键，第二个参数是一个函数，它返回解析数据或抛出错误的承诺。

正如我们所看到的，获取用户名并在屏幕上打印出来很简单，但是如何知道它何时加载呢？

```
import { useQuery } from 'react-query';
import { getUserProfile } from '@api/profile.ts';export const MyProfile = (): JSX.Element => {const { data: userProfile, **isLoading: isUserProfileLoading** } = useQuery('UserProfile', getUserProfile);return {
**isUserProfileLoading** ?
 (<p> Loading user profile</p >) :
 (<h1>Hello, {userProfile?.name}</h1>)
}}
```

正如我们所看到的，知道用户概要文件何时加载是很简单的。React Query 正在为我们处理这些状态。是的，它也很容易出错。请参见下面的示例。

```
import { useQuery } from 'react-query';
import { getUserProfile } from '@api/profile.ts';export const MyProfile = (): JSX.Element => {const { data: userProfile, isLoading: isUserProfileLoading, **isError: isUserProfileError** } = useQuery('UserProfile', getUserProfile);return {<>
**isUserProfileError** && <p>Something went wrong while fetching</p>
 isUserProfileLoading ?
  (<p> Loading user profile</p >) :
  (<h1>Hello, {userProfile?.name}</h1>)
</>
}}
```

我们知道 React Query 是一个非常好的库，可以轻松地从服务器获取数据。但是如果需要的话，我们如何重新获取用户配置文件呢？我们可以像处理 isLoading 和 isError 状态一样简单地做到这一点。请参见下面的示例。

```
import { useQuery } from 'react-query';
import { getUserProfile } from '@api/profile.ts';export const MyProfile = (): JSX.Element => {const { data: userProfile, isLoading: isUserProfileLoading, isError: isUserProfileError, **refetch: refetchUserProfile** } = useQuery('UserProfile', getUserProfile);**const onRefetchUserProfile = async(): Promise<void> => {
  await refetchUserProfile();
}**return {
<>
isUserProfileError && <p>Something went wrong while fetching</p>
isUserProfileLoading ?
 (<p> Loading user profile</p >) :
 (<h1>Hello, {userProfile?.name}</h1>)**<button type="button" onClick={onRefetchUserProfile}>
  Re-fetch the user profile
</button>**
</>
}}
```

我们已经看到了一些如何轻松获取和重新获取数据的例子，但是我们如何更新数据呢？为了更新数据，我们需要使用 React 查询突变。请参见下面的示例。

```
import { useQuery, **useMutation** } from 'react-query';
import { getUserProfile, **updateUserProfile** } from '@api/profile.ts';export const MyProfile = (): JSX.Element => {const { data: userProfile, isLoading: isUserProfileLoading, isError: isUserProfileError, refetch: refetchUserProfile} = useQuery('UserProfile', getUserProfile);const onRefetchUserProfile = async(): Promise<void> => {
  await refetchUserProfile();
}**const { mutation: userProfileMutation, isLoading: isUserProfileMutationLoading, isError: isUserProfileMutationError }= useMutation((username: string) =>    
 updateUserProfile(username));****const handleOnFormSubmit = (formEvent): void => {
 const inputValue = formEvent.target.username.value;
 userProfileMutation(inputValue)
}****const componentHasError = isUserProfileError || 
 isUserProfileMutationError;****const isComponentLoading = isUserProfileLoading ||  
 isUserProfileMutationLoading;**return {
<>
 **componentHasError** && <p>Something went wrong while fetching</p>
 **isComponentLoading** ?
  (<p> Loading user profile</p >) :
  (<h1>Hello, {userProfile?.name}</h1>) <button type="button" onClick={onRefetchUserProfile}>
   Re-fetch the user profile
 </button> **<form onSubmit="handleOnFormSubmit">
   <input type="text" name="username" />
   <button type="submit">Save profile data</button>
 </form>** </>
}}
```

如果上面例子中的更新出错，变异的 isError 状态将被设置为 true，这意味着我们可以创建一个 componentHasError 变量来保存获取和更新的错误状态。React Query 甚至使错误处理变得更加容易。

上面的代码有一个问题，我们缓存的用户配置文件数据是错误的，因为我们更新了服务器上的数据，但我们没有使缓存无效。在服务器上更新用户配置文件数据后，我们没有重新获取它。请参见下面的示例，了解如何使缓存无效。

```
import { useQuery, useMutation } from 'react-query';
import { getUserProfile, updateUserProfile } from '@api/profile.ts';export const MyProfile = (): JSX.Element => {const { data: userProfile, isLoading: isUserProfileLoading, isError: isUserProfileError, refetch: refetchUserProfile} = useQuery('UserProfile', getUserProfile);const onRefetchUserProfile = async(): Promise<void> => {
  await refetchUserProfile();
}const { mutation: userProfileMutation, isLoading: isUserProfileMutationLoading, isError: isUserProfileMutationError }= useMutation((username: string) =>    
 updateUserProfile(username));**const** **invalidateUserProfileCache = (): Promise<void> =>  
 queryClient.invalidateQueries('UserProfile');**const handleOnFormSubmit = (formEvent): void => {
 const inputValue = formEvent.target.username.value;
  userProfileMutation(inputValue, **{
    onSuccess: async () => {
      await invalidateUserProfileCache();
    }
  }**)
}const componentHasError = isUserProfileError || 
 isUserProfileMutationError;const isComponentLoading = isUserProfileLoading ||  
 isUserProfileMutationLoading;return {
<>
 componentHasError && <p>Something went wrong while fetching</p>
 isComponentLoading ?
  (<p> Loading user profile</p >) :
  (<h1>Hello, {userProfile?.name}</h1>)<button type="button" onClick={onRefetchUserProfile}>
   Re-fetch the user profile
 </button><form onSubmit="handleOnFormSubmit">
   <input type="text" name="username" />
   <button type="submit">Save profile data</button>
 </form></>
}}
```

正如我们所看到的，使缓存无效很简单。我们只需要告诉 React Query 在变异成功时使缓存失效。好的一面是，当我们使缓存失效时，React Query 会自动从服务器重新获取更新的数据。不需要手动调用重新获取方法。

# **QueryClientProvider**

要使用 React Query，有必要在组件树的顶层添加 **QueryClientProvider** ，或者将范围扩展到应该能够使用 React Query 的组件。提供程序必须提供在组件之间共享的 QueryClient。QueryClient 使缓存可供其他组件使用。所有使用具有相同缓存键的 useQuery 的组件，React Query，将使用来自缓存的数据，而不是再次从 API 获取。您可以在这里阅读关于查询客户端的更多信息。[https://react-query . tan stack . com/reference/query client provider](https://react-query.tanstack.com/reference/QueryClientProvider)

# 小费！

为了提高代码质量并保持代码库的可维护性，我建议创建自定义挂钩来处理查询，而不是直接在组件中使用 react-query。对于上面的例子，我们应该创建一个自定义钩子，命名为类似于 *useUserProfileData。*然后这个定制钩子实现 React 查询。这使得我们的代码库更加健壮，因为我们可以创建一个方法来使我们的查询无效，而不是在我们的应用程序中利用缓存键，并且可能拼错缓存键。

# 结论

React Query 确实为我们处理了许多与获取、更新、缓存和重试相关的挑战。该库使我们不必花费数小时来开发解决数据读取中普遍挑战的解决方案。使用像 React Query 这样的库，我们不需要维护自定义库来获取数据。

React Query 有很好的文档记录，它使新的团队成员很容易获得处理应用程序获取部分所需的知识。

感谢阅读这篇文章，我希望你喜欢它！:-)