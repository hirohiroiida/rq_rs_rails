## リクエストからレスポンスまでの流れについて(GET編)
1.  ユーザーがリクエスト（メソッド：GET、URL：/users）をHTTPサーバに送ってくる。
2.  リクエストがRailsアプリケーション内に入ると、ルーター（routes.rb）に送られる。
3.  ルーターは、/users に対応する コントローラーやアクションを特定する。
4.  アクションが実行され、必要な情報（ユーザー全員のデータ）をとるためにモデルへ命令する。
5.  モデルがデータベースから全レコード（ユーザー全員のデータ）を引き出す。
6.  引き出したユーザー全員のデータは、コントローラ＆アクション内にあるインスタンス変数@usersに渡される。
7.  @usersがビューで展開され、要求されたhtmlを生成し、コントローラにできたhtmlを返す。
8. コントローラーはレスポンスボディー中htmlを入れて、ユーザーに送る。

## リクエストからレスポンスまでの流れについて(POST編)

#### <span style="color: green; ">新規作成ページを表示する</span>
上で書いた GET編の流れで、/tasks/new の URL からビューを表示する。ただし、tasks_controller.rb のnew action内でTaskモデルのカラム（データの収納場所）を作成し、@task に入れる。

```rb
    def new            # tasks_controller.rb ファイル内
        @task = Task.new
    end
```
[![Image from Gyazo](https://i.gyazo.com/7b6f64e3f7a8209614bce5caf858259b.png)](https://gyazo.com/7b6f64e3f7a8209614bce5caf858259b)

---

#### <span style="color: green; ">タスク名(name)を入力する</span>
* @task にできた収納場所にviews/new.html.erbからform_withを使ってタスク名を入力する。
```erb
<h1>新規投稿</h1>             
<%= form_with model: @task do |f| %>
    <div class="field">
        <%= f.label :タスク名 %>
        <%= f.text_field :name %>
    </div>
    <br>
    <%= f.submit :作成 %>
<% end %>
```
[![Image from Gyazo](https://i.gyazo.com/685989681d1062032a6bab679e09c49f.png)](https://gyazo.com/685989681d1062032a6bab679e09c49f)

---
#### <span style="color: green; ">作成ボタンを押す</span>
* routes.rb で method は POST 、 tasks コントローラー create アクション に案内される。
* 以下の処理で tasks コントローラーの create アクション からデータベスに入力したタスク名(name)を保存する。
    * 処理１：新しく作った taskモデルのカラムに入力したパラメーターの title のみ入れて、その後 @task に入れる。
    * 処理２：@task をデータベースへ保存する。
    * 処理３：タスクの詳細画面に遷移する
↓は tasks_controller.rb の createアクション
```rb
    def create
        @task = Task.new(task_params)           # 処理 1
        @task.save                              # 処理 2
        redirect_to tasks_path                  # 処理 3
    end
    
    def task_params
        params.require(:task).permit(:name)    # 処理 1
    end
```
[![Image from Gyazo](https://i.gyazo.com/cbcbf096232f0184b74375530ff8c36c.png)](https://gyazo.com/cbcbf096232f0184b74375530ff8c36c)
index.html.erb ファイル ↓
```erb
<h2>投稿一覧</h2>
<h3>タスク名</h3>
<% @tasks.each do |task| %>
    <%= task.name %>
    <br>
<% end %>
```
name=tasks[name] のようにパラメーターがnameに入っていることを確認できる
[![Image from Gyazo](https://i.gyazo.com/a8d7608be9992b31d347711e0f8e4e01.png)](https://gyazo.com/a8d7608be9992b31d347711e0f8e4e01)

---

#### <span style="color: green; ">タスクの詳細画面に遷移する</span>
* redirect_to tasks_path か URLに/tasks を打ち込み、GET編同様、タスクの詳細画面に遷移する。

[![Image from Gyazo](https://i.gyazo.com/ad51ddec87f94ec435000f066cf1f2e3.png)](https://gyazo.com/ad51ddec87f94ec435000f066cf1f2e3)


## 感想
今まで独学で勉強してきたことは、細かいところを把握せずに勉強していたので、今回の課題はすごく自分にとって勉強になった。
