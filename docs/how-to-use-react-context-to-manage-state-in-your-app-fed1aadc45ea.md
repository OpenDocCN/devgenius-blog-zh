# 如何使用 React 上下文来管理应用程序中的状态。

> 原文：<https://blog.devgenius.io/how-to-use-react-context-to-manage-state-in-your-app-fed1aadc45ea?source=collection_archive---------13----------------------->

![](img/87b863059284f7ddf6e0567f10e973b5.png)

当你创建一个 web 应用程序时，你必须克服的最大挑战之一就是管理状态，使用 react 有几个选项可以做到这一点，其中之一就是使用 React 上下文。我将用一个例子给你解释清楚，这样你就可以开始在你的 web 应用中使用 React Context 了。

假设我们有一个号码，我们需要在我们的 web 应用程序中的任何地方访问该号码，我们将如何做，只需使用下面的代码。

```
***QuantityContext.tsx***import * as React from ‘react’;interface IQuantityState {
  quantity?: number;
}type QuantityActionType = ‘SET_QUANTITY’ | ‘CLEAR_QUANTITY’;interface IQuantityDispatch {
type: WebCartActionType;
payload?: number;
}interface IQuantityContext {
state: IQuantityState;
dispatch: React.Dispatch<IQuantityDispatch>;
}const initialState: IQuantityState= {
quantity: undefined,
};function reducer(state: IQuantityState, action: IQuantityDispatch) {
switch (action.type) {
case ‘SET_QUANTITY’:
return Object.assign({}, state, {
quantity: action.payload.quantity ?? undefined
});
case ‘CLEAR_QUANTITY’:
return Object.assign({}, state, {
quantity: undefined
});
default:
return state;
}
}
export const QuantityDataContext = React.createContext({} as IQuantityContext);
export const useQuantityDataContext = () => React.useContext(QuantityDatContext);
export const QuantityContextProvider: React.FC = ({ children }) => {
const [state, dispatch] = React.useReducer(reducer, initialState);
return (<QuantityContext.Provider value={{ state, dispatch }}>
{children}
</QuantityContext.Provider>
);
};
```

我们在上面创建了 QuantityContext.tsx 文件，现在我们需要用 QuantityContextProvider 包装我们的应用程序，以便我们可以从应用程序中的任何位置访问状态。

```
***App.tsx***export default function App() {
return (
<QuantityContextProvider>
{children}
</QuantityContextProvider>
);
}
```

现在，是时候设置一些值并在 web 应用程序的另一部分中使用这些值了。

```
***Screen.tsx***import { useQuantityDataContext } from '../../context/QuantityContext.tsx';interface IScreenProps {}export const Screen: React.FC<IScreenProps> = () => {const [quantity, setQuantity] = React.useState(0);
const { dispatch } = useQuantityDataContext();React.useEffect(() => {
if (quantity !== 0) {
dispatch({ type: "SET_QUANTITY", payload: { quantity: quantity} 
}); *// set the value here*
} else {
dispatch({ type: "CLEAR_QUANTITY" });
}}, [quantity]) *//clear the value here*return (
<div>
<button title="Increase" onPress={() => setQuantity(quantity)}></button> //increase the value here with  press on button
</div>
)} 
```

我们在 Screen.tsx 上设置值，现在最后一步是从我们应用程序的任何地方读取该值。我们将创建另一个名为 ScreenTwo.tsx 的屏幕，我们将获取状态并从该状态中读取值。

```
**ScreenTwo.tsx**import { useQuantityDataContext } from '../../context/QuantityContext.tsx';interface IScreenTwoProps {}export const ScreenTwo: React.FC<IScreenTwoProps> = () => {const { state } = useQuantityDataContext() //just get the state return (
<div>
{state.quantity} //you can read the value here
</div>
)}
```

现在，您已经准备好编写第一个上下文来管理 web 应用程序中的状态。希望对你有帮助。如果是这样的话，给我一个关注和喜欢:)

我的链接在下面

[AKIN KARAYUN | LinkedIn](https://www.linkedin.com/in/akin-karayun-ab3239bb/)

[阿金·卡拉云](https://devakinkarayun.web.app/)