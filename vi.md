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

**9. Không chuẩn hóa đủ**

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

**12. Không phân tích hiệu năng về tất cả câu truy vấn**

Chủ nghĩa thực dụng thống trị tối cao, đặc biệt là trong thế giới cơ sở dữ liệu. Nếu bạn đang gắn bó với các nguyên tắc đến mức họ đã trở thành một tín điều thì bạn có lẽ đã mắc phải sai lầm. Lấy ví dụ của các truy vấn tổng hợp từ phía trên. Phiên bản tổng hợp có thể trông "đẹp" nhưng hiệu suất của nó là đáng buồn. Một sự so sánh về hiệu suất đáng nhẽ nên kết thúc cuộc tranh luận(nhưng không) nhưng nhiều hơn thế: đưa ra những tầm nhìn không đáng tin ở nơi đầu tiên thì thật nguy hiểm.

**13. Quá phụ thuộc vào UNION ALL và đặc biệt là cấu trúc UNION **

Một UNION trong SQL chỉ đơn thuần nối các tập dữ liệu đồng nhất, có nghĩa là chúng có cùng kiểu và số cột. Sự khác biệt giữa chúng là UNION ALL đơn giản là một kết nối và được ưa thích bất cứ khi nào có thể, trong khi một UNION ngầm sẽ làm một DISTINCT để loại bỏ bản sao trùng lặp.

Các UNION, như DISTINCT, đã được nói đến. Có các ứng dụng hợp lệ. Nhưng nếu bạn thấy mình đang làm rất nhiều, đặc biệt trong các truy vấn phụ, thì có thể bạn đang làm sai. Đó có thể là trường hợp xây dựng truy vấn kém hoặc mô hình dữ liệu được thiết kế kém buộc bạn phải làm những việc như vậys.

Các UNION, đặc biệt khi sử dụng trong các kết nối hoặc các truy vấn phụ phụ thuộc, có thể làm tê liệt cơ sở dữ liệu. Cố gắng tránh chúng bất cứ khi nào có thể.

**14. Sử dùng điều kiện OR trong truy vấn**

Chúng có vẻ vô hại. Nhưng sau tất cả, AND lại ổn. OR cũng nên được như vậy phải không? Sai rồi. Về cơ bản một điều kiện AND **hạn chế** dữ liệu cài đặt trong khi một điều kiện OR **kích thích** điều này nhưng không phải là cách để cho phép chúng tối ưu hóa. Đặc biệt khi các điều kiện OR khác nhau có thể giao cắt, do đó buộc trình tối ưu hóa có hiệu quả để một hoạt động DISTINCT trong kết quả.

Tồi:

... WHERE a = 2 OR a = 5 OR a = 11

Tốt hơn:

... WHERE a IN (2, 5, 11)

Bây giờ, trình tối ưu hoá SQL của bạn có thể biến truy vấn đầu tiên thành thứ hai. Nhưng nó có thể không. Chỉ cần không làm điều đó.

**15. Không thiết kế mô hình dữ liệu của mình để cho phép các giải pháp hiệu suất cao**

Đây là một điểm khó để định lượng. Nó thường được quan sát bởi hiệu quả của nó. Nếu bạn thấy mình đang viết các truy vấn phức tạp cho các tác vụ tương đối đơn giản hoặc các truy vấn để tìm ra thông tin tương đối đơn giản không hiệu quả thì bạn có thể có một mô hình dữ liệu nghèo.

Ở mức độ nào đó thì quan điểm này tóm tắt lại tất cả những điều ở trên nhưng đó cũng là một câu cảnh báo rằng làm những thứ như tối ưu truy vấn thường được xếp thứ nhất nhưng thực tế nó lại nên xếp thứ 2. Đầu tiên và trước hết bạn nên đảm bảo là bạn có một mô hình dữ liệu tốt trước khi cố gắng tối ưu hiệu suất. Như Knuth đã nói:

> Tối ưu hóa sớm là gốc rễ của tất cả các điều rắc rối

**16. Sử dụng các giao dịch cơ sở dữ liệu không chính xác**

Tất cả các dữ liệu thay đổi cho một quá trình cụ thể phải là nguyên tử. I E. Nếu hoạt động thành công, nó sẽ làm đầy đủ. Nếu không thành công, dữ liệu sẽ không thay đổi. - Không nên có những thay đổi "nửa đã hoàn thành".

Lý tưởng nhất, cách đơn giản nhất để đạt được điều này là toàn bộ thiết kế hệ thống nên cố gắng hỗ trợ tất cả các thay đổi dữ liệu thông qua các câu lệnh đơn INSERT / UPDATE / DELETE. Trong trường hợp này, không có xử lý giao dịch đặc biệt là cần thiết, như một cơ cơ sở dữ liệu của bạn nên làm như vậy tự động.

Tuy nhiên, nếu bất kỳ quy trình nào yêu cầu nhiều lệnh được thực hiện như một đơn vị để giữ dữ liệu ở trạng thái nhất quán, thì cần điều khiển giao dịch thích hợp.

- Bắt đầu Giao dịch trước câu lệnh đầu tiên.
- Cam kết Giao dịch sau câu lệnh cuối cùng.
- Trong bất kì hoàn cảnh nào, hãy phục hồi giao dịch. Đừng quên skip/abort tất cả câu lệnh theo lỗi đó.

Cũng nên chú ý cẩn thận đến các subtelties của lớp kết nối cơ sở dữ liệu của bạn, và công cụ cơ sở dữ liệu tương tác trong vấn đề này.. 

**17. Không hiểu mô hình 'set-based'**

Ngôn ngữ SQL tuân theo một mô hình phù hợp với từng trường hợp cụ thể. Hàng tá các vendor-specific extension là khác nhau, ngôn ngữ này phải vất vả để giải quyết các vấn đề tầm thường trong ngôn ngữ langues như Java, C #, Delphi.

Sự thiếu hiểu biết này thể hiện theo một vài cách.

- Áp dụng không chính đáng quá nhiều quy tắc hoặc bắt buộc trên vùng dữ liệu.
- Việc sử dụng không phù hợp hoặc quá mức các con trỏ. Đặc biệt là khi một truy vấn đơn là đủ.
- Giả định không đúng kích hoạt một lần mỗi hàng bị ảnh hưởng trong cập nhật nhiều hàng.

Xác định phân chia trách nhiệm rõ ràng và cố gắng sử dụng công cụ thích hợp để giải quyết từng vấn đề.
