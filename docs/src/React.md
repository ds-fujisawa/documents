# Reactメモ

# ~WIP~

## [公式](https://reactjs.org/)

## 概要

FacebookがOSSとして公開しているUIを構築するためのJavaScriptライブラリ

* 宣言的
* コンポーネントベース
* 一度学んで、どこでも書く

## 始め方

* Local
	* [create-react-app](https://facebook.github.io/create-react-app/)
	* [CDN](https://raw.githubusercontent.com/reactjs/reactjs.org/master/static/html/single-file-example.html)
	* [個人的に作成したもの](https://github.com/ds-fujisawa/react-start)
* Online
	* [CodePen](https://reactjs.org/redirect-to-codepen/hello-world)
	* [CodeSandbox](https://codesandbox.io/s/new)

## コンポーネント

**Props**

**State**

**setState()**

**StatelessFunctionalComponent**


## ライフサイクル

* **マウントに関するメソッド**
	* constructor
	* componentWillMount ※
	* static getDerivedStateFromProps
	* render
	* componentDidMount
* **アップデートに関するメソッド**
	* componentWillReceiveProps ※
	* static getDerivedStateFromProps
	* shouldComponentUpdate
	* componentWillUpdate ※
	* getSnapshotBeforeUpdate
	* componentDidUpdate
* **アンマウントに関するメソッド**
	* componentWillUnmount
* **エラーハンドリングに関するメソッド**
	* componentDidCatch

**末尾に※が付いているものは今後のバージョンアップで削除予定**

**詳しくは**

* [公式](https://reactjs.org/docs/react-component.html#the-component-lifecycle)
* [Qiitaで個人的にまとめたもの](https://qiita.com/f-a24/items/40b83d4c6c7d147cda9e)

## Code-Splitting

	* React.lazy
	* Suspense

## Context

	* React.createContext
	* Context.Provider
	* Class.contextType
	* Context.Consumer

## Hooks（試験段階）

* **Basic Hooks**
	* useState
	* useEffect
	* useContext
* **Additional Hooks**
	* useReducer
	* useCallback
	* useMemo
	* useRef
	* useImperativeMethods
	* useLayoutEffect
