## Những lỗi phát triển cơ sở dữ liệu phổ biến của các nhà phát triển ứng dụng là gì ?

_nguồn https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers_

**1. Không sử dụng các chỉ số thích hợp**

Đây là một cái tương đối đơn giản nhưng vẫn hay xảy xa mọi lúc. Các khóa ngoại nên có chỉ mục trên chúng. Nếu bạn đang sử dụng một trường trong mệnh đề WHERE bạn nên chắc chắn có một chỉ số trong chúng. Các chỉ mục này thường bao gồm nhiều cột dựa trên các truy vấn bạn cần để thực hiện.

**2. Không thực thi toàn vẹn tham chiếu**

Cơ sở dữ liệu của bạn có thể thay đổi ở đây nhưng nếu cơ sở dữ liệu của bạn hỗ trợ tính toàn vẹn tham chiếu - có nghĩa là tất cả các khoá ngoại được đảm bảo để trỏ đến một thực thể tồn tại - bạn nên sử dụng nó

Rất phổ biến để thấy sự thất bại này trên cơ sở dữ liệu MySQL. Tôi không tin rằng MyISAM hỗ trợ nó. Nhưng InnoDB thì có. Bạn sẽ tìm thấy những người đang sử dụng MyISAM hoặc những người đang sử dụng InnoDB nhưng không sử dụng nó anyway.

Đọc thêm:

- [How important are constraints like NOT NULL and FOREIGN KEY if I’ll always control my database input with php?](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)
- [Are foreign keys really necessary in a database design?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)
- [Are foreign keys really necessary in a database design?](http://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)

**3. Sử dụng khóa tự nhiên thay vì (kỹ thuật) khóa chính đại diện**

Khóa tự nhiên là các khóa dựa trên các dữ liệu có nghĩa bên ngoài mà (có vẻ) độc nhất. Các ví dụ phổ biến là mã sản phẩm, mã bưu điện gồm hai chữ cái (US), số an sinh xã hội. Các khóa chính thay thế hoặc kỹ thuật chính là những khoá hoàn toàn không có ý nghĩa bên ngoài hệ thống. Chúng được phát minh hoàn toàn để xác định thực thể và thông thường các trường tự động gia tăng (SQL Server, MySQL, và những cái khác) hoặc các chuỗi (nhất là Oracle).

Quan điểm của tôi là bạn nên **luôn** sử dụng khóa chính đại diện. Vấn đề này đã đưa ra trong những câu hỏi sau:

- [How do you like your primary keys?](https://stackoverflow.com/questions/404040/how-do-you-like-your-primary-keys)
- [What's the best practice for primary keys in tables?](https://stackoverflow.com/questions/337503/whats-the-best-practice-for-primary-keys-in-tables)
- [Which format of primary key would you use in this situation.](https://stackoverflow.com/questions/506164/which-format-of-primary-key-would-you-use-in-this-situation)
- [Surrogate vs. natural/business keys](https://stackoverflow.com/questions/63090/surrogate-vs-natural-business-keys)
- [Should I have a dedicated primary key field?](https://stackoverflow.com/questions/166750/should-i-have-a-dedicated-primary-key-field)

Đây là một chủ đề gây nhiều tranh cãi mà bạn sẽ không nhận được thỏa thuận chung. Trong khi bạn có thể tìm thấy một số người, những người nghĩ rằng các khóa tự nhiên trong một số trường hợp OK, bạn sẽ không tìm thấy bất kỳ lời chỉ trích của các khóa đại diện khác hơn là cho là không cần thiết. Đó là một nhược điểm nhỏ nếu bạn hỏi tôi.

Hãy nhớ rằng, ngay cả [các quốc gia có thể không còn tồn tại](http://en.wikipedia.org/wiki/ISO_3166-1) (ví dụ, Yugoslavia).

**4. Viết các truy vấn yêu cầu 

để DISTINCT hoạt động**

Bạn thường thấy điều này trong các truy vấn được tạo ra bởi ORM. Xem kết xuất nhật ký từ Hibernate và bạn sẽ thấy tất cả các truy vấn bắt đầu bằng:

SELECT DISTINCT ...

Sẽ mất 1 lúc để đảm bảo bạn không tạo ra các dòng bị trùng và dẫn đến các đối tượng bị trùng. Bạn sẽ thỉnh thoảng thấy vài người làm như vậy. Nếu bạn thấy điều này quá nhiều thì thực sự đáng báo động đấy.Không phải do DISTINCE tệ hay không có các ứng dụng hợp lệ. Nó tốt (cả 2 mặt) nhưng nó không phải đại diện hoặc tạm thời để viết các câu truy vấn đúng.

Từ [Tại sao tôi ghét DISTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):

>Trong trường hợp mọi thứ bắt đầu trở nên chua theo quản điểm ​​của tôi là khi một nhà phát triển đang xây dựng truy vấn đáng kể, tham gia các bảng với nhau, và đột nhiên ông nhận ra rằng có vẻ như ông đang nhận được bản sao (hoặc thậm chí nhiều hơn) hàng và phản ứng ngay lập tức của ông "giải pháp" của anh ta đối với "vấn đề" này là ném vào từ khoá DISTINCT và **POOF** tất cả những rắc rối của anh ấy biến mất.

**5. Khuyến khích tập hợp các kết nối**

Một sai lầm phổ biến khác của các nhà phát triển ứng dụng cơ sở dữ liệu là không nhận ra sự kết hợp  ( Mệnh đề GROUP BY) tốn kém hơn so sánh với phép nối..

Để cung cấp cho bạn một ý tưởng về cách phổ thông này, tôi đã viết về chủ đề này nhiều lần ở đây và được downvoted rất nhiều cho nó. Ví dụ:

Từ [SQL statement - “join” vs “group by and having”](https://stackoverflow.com/questions/477006/sql-statement-join-vs-group-by-and-having/477013#477013):

> Câu truy vấn thứ nhất:

SELECT userid
FROM userrole
WHERE roleid IN (1, 2, 3)
GROUP by userid
HAVING COUNT(1) = 3

> Query time: 0.312 s

> Câu truy vấn thứ hai:

SELECT t1.userid
FROM userrole t1
JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2
JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3
AND t1.roleid = 1

> Query time: 0.016 s

> Đúng vậy. Phiên bản nối tôi đề xuất **nhanh gấp 2 lần phiên bản nhóm**

**6. Không đơn giản hóa các truy vấn phức tạp thông qua các chế độ xem**

Không phải tất cả các nhà cung cấp cơ sở dữ liệu hỗ trợ quan điểm nhưng đối với những người làm, họ có thể rất đơn giản hóa các truy vấn nếu được sử dụng một cách thận trọng. Ví dụ, trong một dự án tôi đã dùng [generic Party model](http://www.tdan.com/view-articles/5014/) cho CRM. Đây là một kỹ thuật mô hình rất mạnh và linh hoạt nhưng có thể dẫn đến nhiều phép nối. Trong mô hình này có:

- **Party**: người và tổ chức;
- **Party Role**: những điều mà các bên đã làm, ví dụ như Nhân viên và Người sử dụng lao động;
- **Party Role Relationship**: làm thế nào những vai trò liên quan đến nhau.

Ví dụ:

- Ted là 1 Person, là 1 tập con Party;
- Ted có nhiều luật, 1 trong số chúng là Employee;
- Intel là 1 tổ chức, là 1 tập con cửa Party;
- Intel có nhiều luật, 1 trong số chúng là Employer;
- Intel tuyển Ted, nghĩa là có quan hệ giữa các luật tương ứng của chúng.

Do vậy, có 5 bảng nối để kếtết nối Ted với các nhà tuyển dụng của anh ý.Giả định rằng các nhân viên là Persons(không phải các tổ chức) và cung cấp các view hỗ trợ:

CREATE VIEW vw_employee AS
SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name
FROM person p
JOIN party py ON py.id = p.id
JOIN party_role child ON p.id = child.party_id
JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT'
JOIN party_role parent ON parent.id = prr.parent_id = parent.id
JOIN party p2 ON parent.party_id = p2.id

Và bỗng nhiên bạn có 1 view rất đơn giản các dữ liệu bạn muốn những ở mô hình dữ liệu linh hoạt cao.

**7. Không lọc đầu vào**

Đây là 1 chuyện lớn đấy. Bây giờ tôi thích PHP nhưng nếu bạn không muốn biết bạn đang làm gì nó thực sự dễ dàng trong việc tạo các site có thể bị tấn công. Không cái nào tổng kết điều này tốt hơn là  [câu chuyện của Bobby Tables bé nhỏ](http://xkcd.com/327/).

Dữ liệu được cung cấp bởi người sử dụng bằng các URL, hình thức dữ liệu và cookie nên luôn được coi là có tính thù địch và lọc. Đảm bảo bạn đang nhận được những gì bạn mong đợi.

**8. Không sử dụng các câu lệnh chuẩn bị**

Các câu lệnh chuẩn bị là khi bạn biên dịch 1 truy vấn khuyết dữ liệu sử dụng trong các lệnh thêm, cập nhật và

mệnh đề WHERE và sẽ cung cấp dữ liệu cho nó sau. Ví dụ:

SELECT * FROM users WHERE username = 'bob'

vs

SELECT * FROM users WHERE username = ?

or

SELECT * FROM users WHERE username = :username

dựa trên nền tảng của bạn.

Tôi đã từng thấy các cơ sở dữ liệu bị phá hủy vì điều này. Về cơ bản, mỗi khi cơ sở dữ liệu hiện đại gặp một truy vấn mới, nó phải biên dịch nó. Nếu nó gặp một truy vấn nó được nhìn thấy trước, bạn đang cho cơ sở dữ liệu cơ hội để cache truy vấn biên dịch và kế hoạch thực hiện. Bằng cách thực hiện các truy vấn rất nhiều bạn đang cho cơ sở dữ liệu cơ hội để tìm kiếm và tối ưu hóa cho phù hợp (ví dụ, bằng cách ghim các truy vấn biên dịch trong bộ nhớ).

Sử dụng các câu lệnh chuẩn bị cũng sẽ cung cấp cho bạn số liệu thống kê có ý nghĩa về tần suất các truy vấn nhất định được sử dụng.

Các câu lệnh được chuẩn bị cũng bảo vệ bạn tốt hơn với cách tấn công SQL injection.

**9. Không đủ chuẩn hóa**

[Chuẩn hóa cơ sở dữ liệu](http://en.wikipedia.org/wiki/Database_normalization) về cơ bản là quá trình tối ưu hóa thiết kế cơ sở dữ liệu hoặc cách bạn tổ chức dữ liệu của bạn vào các bảng.

 Tuần này tôi bắt gặp code của ai đó mà họ tách 1 mảng ra và thêm chúng vào 1 trường đơn lẻ trong 1 cơ sở dữ liệu. Quả trình chuẩn hóa nên là coi các phần tử của mảng như là 1 dòng riêng biệt trong bảng con (ví dụ quan hệ một-nhiều) .

Điều này cũng xuất hiện trong [Phương pháp tốt nhất để lưu trữ user IDs](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):

> Tôi đã nhìn thấy trong các hệ thống khác mà danh sách được lưu trữ trong một mảng PHP tuần tự.

Tuy nhiên, sự thiếu chuẩn hóa lại có nhiều hình thức.

Thêm:

- [Normalization: Bao nhiêu thì đủ?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
- [SQL qua Design: Tại sao bạn cần phải chuẩn hóa cơ sở dữ liệu ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)

**10. Chuẩn hóa quá nhiều**

Điều này có vẻ như mâu thuẫn với điểm trước nhưng như nhiều thứ khá, chuẩn hóa là một công cụ. Nó sẽ chẳng có một điểm kết thúc cụ thể nào cả. Tôi nghĩ rằng nhiều nhà phát triển quên điều này và bắt đầu coi một "công cụ" như là một "kết thúc".Kiểm thử đơn vị là một ví dụ cơ bản cho điều này.

Tôi đã từng làm việc trên một hệ thống có một hệ thống phân cấp rất lớn cho khách hàng mà đã gặp một điều gì đó như:

Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...

như vậy bạn phải tham gia cùng với khoảng 11 bảng với nhau trước khi bạn có thể có được bất kỳ dữ liệu có ý nghĩa. Đó là một ví dụ điển hình về quá trình bình chuẩn hóa quá nhiều.

Thêm vào đó, việc tiêu chuẩn hóa lại 1 cách cẩn thận và được lưu tâm có thể mang lại những lợi ích đáng kể nhưng bạn phải thực sự cẩn thận khi làm điều này.

Đọc thêm:

- [Tại sao chuẩn hóa quá nhiều cơ sở dữ liều là một điều tồi](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
- [Làm thế nào để có chuẩn hóa trong thiết kế cơ sở dữ liệu?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
- [KHi không chuẩn hóa cơ sở dữ liệu SQL của bạn](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
- [Có thể chuẩn hóa không chuẩn](http://www.codinghorror.com/blog/archives/001152.html)
- [Nguyên nhân các cuộc tranh luận chuẩn hóa cơ sở dữ liệu trên Coding Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)

**11. Sử dụng exclusive arcs**

Một exclusive arcs là một sai lầm phổ biến nơi một bảng được tạo ra với hai hoặc nhiều khóa ngoại nơi mà một và chỉ một trong số họ có thể không null.  **Sai lầm lớn.** với một điều nó trở nên khó khăn hơn nhiều để duy trì tính toàn vẹn dữ liệu.Sau tất cả, ngay cả với tính toàn vẹn tham chiếu, không có gì ngăn ngừa hai hoặc nhiều hơn các khóa ngoại được thiết lập (khó khăn kiểm tra phức tạp dù vẫn có).

Từ [Hướng dẫn Thực tiễn về Thiết kế cơ sở dữ liệu quan hệ](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):

> Chúng tôi đã tư vấn mạnh mẽ chống lại việc xây dựng exclusive bất cứ khi nào có thể, vì lý do là có thể khó viết mã và gây ra nhiều khó khăn về bảo trì.

**12. Không phân tích hiệu suất về các truy vấn ở tất cả**

Chủ nghĩa thực dụng thống trị tối cao, đặc biệt là trong thế giới cơ sở dữ liệu. If you're sticking to principles to the point that they've become a dogma then you've quite probably made mistakes. Take the example of the aggregate queries from above. The aggregate version might look "nice" but its performance is woeful. A performance comparison should've ended the debate (but it didn't) but more to the point: spouting such ill-informed views in the first place is ignorant, even dangerous.

**13. Over-reliance on UNION ALL and particularly UNION constructs**

A UNION in SQL terms merely concatenates congruent data sets, meaning they have the same type and number of columns. The difference between them is that UNION ALL is a simple concatenation and should be preferred wherever possible whereas a UNION will implicitly do a DISTINCT to remove duplicate tuples.

UNIONs, like DISTINCT, have their place. There are valid applications. But if you find yourself doing a lot of them, particularly in subqueries, then you're probably doing something wrong. That might be a case of poor query construction or a poorly designed data model forcing you to do such things.

UNIONs, particularly when used in joins or dependent subqueries, can cripple a database. Try to avoid them whenever possible.

**14. Using OR conditions in queries**

This might seem harmless. After all, ANDs are OK. OR should be OK too right? Wrong. Basically an AND condition **restricts** the data set whereas an OR condition **grows** it but not in a way that lends itself to optimisation. Particularly when the different OR conditions might intersect thus forcing the optimizer to effectively to a DISTINCT operation on the result.

Bad:

... WHERE a = 2 OR a = 5 OR a = 11

Better:

... WHERE a IN (2, 5, 11)

Now your SQL optimizer may effectively turn the first query into the second. But it might not. Just don't do it.

**15. Not designing their data model to lend itself to high-performing solutions**

This is a hard point to quantify. It is typically observed by its effect. If you find yourself writing gnarly queries for relatively simple tasks or that queries for finding out relatively straightforward information are not efficient, then you probably have a poor data model.

In some ways this point summarizes all the earlier ones but it's more of a cautionary tale that doing things like query optimisation is often done first when it should be done second. First and foremost you should ensure you have a good data model before trying to optimize the performance. As Knuth said:

> Premature optimization is the root of all evil

**16. Incorrect use of Database Transactions**

All data changes for a specific process should be atomic. I.e. If the operation succeeds, it does so fully. If it fails, the data is left unchanged. - There should be no possibility of 'half-done' changes.

Ideally, the simplest way to achieve this is that the entire system design should strive to support all data changes through single INSERT/UPDATE/DELETE statements. In this case, no special transaction handling is needed, as your database engine should do so automatically.

However, if any processes do require multiple statements be performed as a unit to keep the data in a consistent state, then appropriate Transaction Control is necessary.

- Begin a Transaction before the first statement.
- Commit the Transaction after the last statement.
- On any error, Rollback the Transaction. And very NB! Don't forget to skip/abort all statements that follow after the error.

Also recommended to pay careful attention to the subtelties of how your database connectivity layer, and database engine interact in this regard. 

**17. Not understanding the 'set-based' paradigm**

The SQL language follows a specific paradigm suited to specific kinds of problems. Various vendor-specific extensions notwithstanding, the language struggles to deal with problems that are trivial in langues like Java, C#, Delphi etc.

This lack of understanding manifests itself in a few ways.

- Inappropriately imposing too much procedural or imperative logic on the databse.
- Inappropriate or excessive use of cursors. Especially when a single query would suffice.
- Incorrectly assuming that triggers fire once per row affected in multi-row updates.

Determine clear division of responsibility, and strive to use the appropriate tool to solve each problem.
