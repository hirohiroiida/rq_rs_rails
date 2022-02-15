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
1. ユーザーがHTTPリクエスト（メソッド：GET、URL：/tasks/new）をサーバに送ってくる。
2. GET、/tasks/newの情報を基にroutes.rbからコントローラの new actionを実行する。
3. インスタンス変数@taskに空のTaskオブジェクトが代入される。
4. views/new.html.erb に書かれている form_with が@taskを元にモデルオプションで フォームの要素の部分をよしなに出力する。そのおかげで新規作成画面の html が作成される。

```rb
    def new            # tasks_controller.rb ファイル内
        @task = Task.new
    end
```

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

[![Image from Gyazo](https://i.gyazo.com/7b6f64e3f7a8209614bce5caf858259b.png)](https://gyazo.com/7b6f64e3f7a8209614bce5caf858259b)

---

#### <span style="color: green; ">タスク名(name)を入力する</span>
5. ユーザーが新規作成画面でタスク名を入力する。


[![Image from Gyazo](https://i.gyazo.com/5307eb9d4ce56d70be9273661a2c6aa8.png)](https://gyazo.com/5307eb9d4ce56d70be9273661a2c6aa8)

---
#### <span style="color: green; ">作成ボタンを押す</span>
6. form_withでよしなにできたinput要素に入力した情報は、これまたよしなにできた action="/tasks"、method="post"により 入力情報を/tasksというURLにPOSTのリクエストを送る。
7. リクエストは POST、/tasksの情報を基にroutes.rbからコントローラの create actionを実行する。

以下の処理で tasks コントローラーの create アクション からデータベスに入力したタスク名(name)を保存する。

8. 処理１：新しく作った taskモデルのカラムに、 task_paramsメソッドで取得した情報入れた Taskオブジェクトを作り、 それを @task に入れる。
task_params は先ほど送られてきた情報の中で、task に関する name の情報だけを抜き取るメソッド。


9. 処理２：@task をデータベースへ保存する。
10. 処理３：タスクの詳細画面にリダイレクトする

↓は tasks_controller.rb の createアクション
```rb
    def create
        @task = Task.new(task_params)           # 処理 1
        @task.save                              # 処理 2
        redirect_to "/tasks/#{@task.id}"        # 処理 3
    end
    
    def task_params
        params.require(:task).permit(:name)    # 処理 1
    end
```

#### <span style="color: green; ">タスクの詳細画面に遷移する</span>
* リダイレクトにより、タスクの詳細画面に遷移する。

show.html.erb ファイル ↓
```erb
<h1>詳細画面</h1>

<%= @task.name %>
```
[![Image from Gyazo](https://i.gyazo.com/2b68dde6d8790f0e64a9082ab6d52c11.png)](https://gyazo.com/2b68dde6d8790f0e64a9082ab6d52c11)

[![Image from Gyazo](https://i.gyazo.com/9635c9b13cd45a885d96e8d4ae522629.png)](https://gyazo.com/9635c9b13cd45a885d96e8d4ae522629)


# 感想
今まで独学で勉強してきたことは、細かいところを把握せずに勉強していたので、今回の課題はすごく自分にとって勉強になった。
