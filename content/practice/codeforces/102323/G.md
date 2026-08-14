---
title: "CF 102323G - Đĩa bẩn"
description: "Chúng tôi có ba ngăn xếp. Ngăn xếp bẩn ban đầu chứa hoán vị 1..n. Thao tác loại 1 sẽ loại bỏ từ 1 đến một tấm từ trên cùng của ngăn xếp bẩn và đặt chúng vào ngăn xếp trung gian mà không thay đổi thứ tự bên trong của chúng."
date: "2026-08-14T00:40:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102323
codeforces_index: "G"
codeforces_contest_name: "UCF Locals 2014"
rating: 0
weight: 102323
solve_time_s: 248
verified: true
draft: false
---

[CF 102323G - Đĩa bẩn](https://codeforces.com/problemset/problem/102323/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có ba ngăn xếp. Ngăn xếp bẩn ban đầu chứa một hoán vị của`1..n`. một loại`1`hoạt động loại bỏ giữa`1`Và`a`các tấm từ trên cùng của ngăn xếp bẩn và đặt chúng lên ngăn xếp trung gian mà không thay đổi thứ tự bên trong của chúng. một loại`2`hoạt động loại bỏ giữa`1`Và`b`các tấm từ trên cùng của ngăn xếp trung gian và đặt chúng vào máy sấy, một lần nữa giữ nguyên trật tự bên trong của chúng. 

Máy sấy cuối cùng phải được sắp xếp theo kích thước tấm. Vì đầu vào mô tả ngăn xếp bẩn từ trên xuống dưới nên nhiệm vụ không chỉ đơn thuần là sắp xếp hoán vị bằng các hoán đổi tùy ý. Sự sắp xếp lại duy nhất có thể xảy ra là liên tục lấy các tiền tố của một ngăn xếp và đặt các tiền tố đó vào một ngăn xếp khác. 

Những hạn chế là`n <= 2000`, trong khi`a`Và`b`nhiều nhất cũng có`n`. Một người bình thường`O(n^2)`mô phỏng ở đây là hoàn toàn hợp lý, trong khi việc liệt kê các chuỗi thao tác tùy ý là theo cấp số nhân. Khó khăn trọng tâm không phải là tìm ra cấu trúc dữ liệu nhanh mà là chứng minh thao tác nào là bắt buộc khi các ngăn xếp hiện tại chưa thể đạt được tiến bộ. 

Một cách hữu ích để xem xét tình huống là xem xét các kích cỡ đĩa liên tiếp. Giả sử hai tấm lân cận trong ngăn xếp trung gian có kích thước`x`Và`y`, từ trên xuống dưới và`y < x - 1`. Khi đó kích thước còn thiếu giữa chúng không bao giờ có thể được chèn vào. Cả hai tấm chỉ có thể rời khỏi ngăn xếp trung gian từ trên cùng của nó, vì vậy không có hoạt động nào trong tương lai có thể đặt tấm bị thiếu vào giữa chúng. Trạng thái như vậy vĩnh viễn là không thể được. 

Điều này đưa ra khái niệm chính về sự bế tắc. Ví dụ, với```
n = 3, a = 3, b = 3
2 1 3
```Hai tấm đầu tiên có thể được chuyển sang ngăn xếp trung gian, nhưng việc đặt chúng không đúng thứ tự sẽ tạo ra một khoảng trống mà sau này không thể sửa chữa được. Một mô phỏng bất cẩn chỉ kiểm tra xem tấm mong muốn tiếp theo có ở đâu đó trong một trong các ngăn xếp hay không có thể chấp nhận trạng thái như vậy mặc dù điều đó là không thể. 

Một trường hợp ranh giới khác là đầu vào được sắp xếp hoàn toàn. Ví dụ,```
7 7 7
1 2 3 4 5 6 7
```có thể được giải quyết bằng cách chuyển tất cả bảy tấm sang ngăn xếp trung gian và sau đó cả bảy tấm vào máy sấy. Sự thật là`a`Và`b`có thể bằng`n`có nghĩa là việc triển khai phải cho phép một thao tác tiêu thụ toàn bộ ngăn xếp còn lại. 

Ở thái cực đối lập,```

```cũng có thể giải được, nhưng chỉ bằng cách di chuyển từng tấm một. Một giải pháp giả định rằng mỗi lần chuyển giao hữu ích sẽ lấy số lượng đĩa lớn nhất có thể sẽ từ chối trường hợp này một cách không chính xác. 

Cuối cùng, giới hạn năng lực áp dụng riêng cho hai hoạt động. Một chuỗi có thể hoàn toàn hợp lệ với dung lượng không giới hạn nhưng không thể khi`a`quá nhỏ để tạo cấu hình trung gian cần thiết hoặc khi`b`quá nhỏ để loại bỏ khối cần thiết khỏi ngăn xếp trung gian. Coi hai giới hạn là một giới hạn chung cũng là một nguồn dễ dẫn đến nghiệm sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi hoạt động hợp pháp có thể có ở mọi tiểu bang. Từ ngăn xếp bẩn có tới`a`các lựa chọn về số lượng đĩa được di chuyển và từ ngăn xếp trung gian có tới`b`sự lựa chọn. Ngay cả đối với một hoán vị ngắn, các lựa chọn khác nhau sẽ tạo ra các ngăn xếp trung gian khác nhau, do đó số lượng chuỗi thao tác có thể tăng theo cấp số nhân. Với`n = 2000`, điều này không khả thi từ xa. 

Điểm khởi đầu hữu ích hơn là vấn đề với`a = b = infinity`. Không thể sửa đổi máy sấy sau khi đặt một tấm ở đó, vì vậy bất cứ khi nào phần trên cùng của ngăn xếp trung gian có thể được đặt vào đúng vị trí của nó, việc làm như vậy ngay lập tức luôn an toàn. Điều tương tự cũng áp dụng cho ngăn xếp bẩn, ngoại trừ việc tấm đĩa trước tiên phải đi qua ngăn xếp trung gian. 

Sau khi tất cả các vị trí có thể ngay lập tức đã được thực hiện, hãy xem xét tiền tố dài nhất của ngăn xếp bẩn có dạng sau. Nó bao gồm các lần chạy liên tiếp tăng dần và mỗi lần chạy sau chứa các giá trị nhỏ hơn mỗi lần chạy trước đó. Ví dụ,```

```là một chuỗi như vậy, với các khối`[6,7]`,`[3,4]`,`[1,2]`. 

Gọi đây là dãy gần như giảm dần. Nếu chúng ta di chuyển các khối của nó từ trên xuống dưới vào ngăn xếp trung gian, các khối sẽ bị đảo ngược, tạo ra```

```không chứa khoảng trống và có thể tương tác an toàn với các khối nhỏ hơn trong tương lai. 

Lý do tiền tố này quan trọng là vì việc lấy bất kỳ tấm nào ra bên ngoài nó sẽ đặt hai giá trị có khoảng trống không thể lấp đầy cạnh nhau trong ngăn xếp trung gian. Do đó, khi tất cả các vị trí ngay lập tức đã được thực hiện, bước di chuyển có ý nghĩa tiếp theo về cơ bản bị ép buộc bởi tiền tố tối đa này. 

Nếu các giá trị trong tiền tố đó chứa lỗ hổng thì sẽ không có tính linh hoạt. Toàn bộ tiền tố phải được di chuyển theo một loại`1`hoạt động. Nếu chiều dài của nó vượt quá`a`, câu trả lời là không thể. 

Nếu không có lỗ trống, tiền tố bao gồm các giá trị liên tiếp và có thể được chia thành các khối liên tiếp. Khi đủ dung lượng, việc di chuyển các khối đó theo thứ tự sẽ đảo ngược thứ tự của chúng trong ngăn xếp trung gian và duy trì cấu trúc liên tiếp cần thiết. 

Trường hợp tế nhị duy nhất là khi tiền tố bao gồm chính xác hai khối và một trong các khối có dung lượng quá nhỏ để truyền khối trực tiếp. Trong tình huống đó, chỉ có hai cách khả thi để phân chia quá trình chuyển tiền trong khi tránh bế tắc: chia khối trên hoặc chia khối dưới. Điểm phân chia liên quan có thể được kiểm tra trực tiếp dựa trên hai giới hạn dung lượng. Với nhiều hơn hai khối, sự phân chia nhất thiết phải tạo ra một khoảng trống vĩnh viễn ở đâu đó, do đó không có sự sắp xếp thay thế nào tồn tại. 

Điều này biến vấn đề ban đầu từ một tìm kiếm khổng lồ trên các chuỗi hoạt động thành một mô phỏng xác định với một số lượng nhỏ các trường hợp cục bộ. Giải pháp tham khảo có thể thực hiện kiểm tra cục bộ trong`O(n^2)`, thật thoải mái bên trong`n <= 2000`ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô phỏng kinh điển |`O(n^2)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì ba ngăn xếp một cách rõ ràng, cùng với trình tự thao tác. Luôn thực hiện mọi vị trí hiện có có thể vào máy sấy trước khi đưa ra quyết định mới. Một tấm đã có thể rời khỏi ngăn xếp trung gian sẽ không thể giúp ích gì cho chúng ta sau này, vì vậy việc trì hoãn nó chỉ khiến trạng thái trở nên khó giải quyết hơn. 
2. Nếu ngăn xếp bẩn trống và ngăn xếp trung gian cũng trống, tất cả các tấm đã được đặt và quá trình xây dựng thành công. 
3. Tìm tiền tố tối đa của ngăn xếp bẩn gần như giảm dần. Quét các phần tử liên tiếp và tách tiền tố bất cứ khi nào giá trị tiếp theo không lớn hơn chính xác một giá trị so với giá trị hiện tại, đồng thời kiểm tra xem phần đầu của các khối liên tiếp có giảm hay không. 
4. Kiểm tra xem các giá trị chứa trong tiền tố này có tạo thành một khoảng liên tục hay không. Nếu không, tiền tố có chứa một lỗ hổng. Trong trường hợp đó, không có cách nào an toàn để phân chia nó, vì vậy toàn bộ tiền tố phải được di chuyển theo một loại`1`hoạt động. Nếu chiều dài của nó vượt quá`a`, việc xây dựng là không thể. 
5. Nếu tiền tố không có lỗ, hãy xác định các khối liên tiếp của nó. Di chuyển từng khối theo thứ tự hiện tại sẽ đảo ngược thứ tự của chúng trên ngăn xếp trung gian. Vì các giá trị khối là liên tiếp và bản thân các khối được sắp xếp theo giá trị giảm dần nên chuỗi trung gian thu được không có bế tắc. 
6. Nếu mọi khối có thể được chuyển trong`a`, thực hiện các chuyển giao đó. Thứ tự chuyển giao rất quan trọng vì mỗi khối mới được đặt lên trên ngăn xếp trung gian. 
7. Nếu một khối quá lớn`a`, phân biệt trường hợp một khối và hai khối. Một khối đơn lẻ có thể được chuyển từ tấm này sang tấm khác vì việc đảo ngược một khối liên tiếp sẽ không tạo ra giá trị bị thiếu. Với nhiều hơn hai khối, việc tách một trong số chúng nhất thiết sẽ tạo ra khoảng trống ở ranh giới khác, do đó không có công trình hợp lệ nào tồn tại. 
8. Đối với chính xác hai khối, hãy thử hai hướng phân chia có thể. Trong một cách sắp xếp, khối đầu tiên được chia thành hai loại`1`hoạt động, và theo cách sắp xếp khác, khối thứ hai được chia. Đối với mỗi ứng viên, hãy xác minh rằng mọi loại`1`chuyển có kích thước tối đa`a`và mọi loại kết quả`2`chuyển có kích thước tối đa`b`. 
9. Sau mỗi loại`1`hoạt động, thực hiện nhiều lần mọi loại`2`hoạt động hiện đang hợp lệ và đặt các tấm tiếp xúc vào vị trí cần thiết của chúng. Vì máy sấy không bao giờ cần phải thay sau khi đặt đĩa vào đó nên hành động tham lam này không thể phá hủy giải pháp. 
10. Tiếp tục cho đến khi tất cả các đĩa đã được xử lý. Nếu đạt đến bế tắc và không có trường hợp chính tắc nào được áp dụng, hãy in`NO`. 

Tính bất biến của tính đúng là sau mỗi lần lặp, ngăn xếp trung gian không chứa khoảng trống không thể sửa chữa được. Bất cứ khi nào một tấm có thể được đặt vào máy sấy, chúng tôi đặt nó ngay lập tức. Nếu không thể đặt tấm nào, tiền tố gần như giảm tối đa sẽ xác định chính xác phần của ngăn xếp bẩn có thể được di chuyển mà không tạo ra bế tắc. Phân tích trường hợp liệt kê tất cả các cách an toàn có thể có để di chuyển tiền tố đó theo hai giới hạn dung lượng. Do đó, mọi trạng thái do thuật toán tạo ra đều có thể mở rộng bất cứ khi nào trường hợp chính tắc tương ứng cho biết và bất cứ khi nào không có trường hợp nào, mọi khả năng tiếp tục sẽ tạo ra bế tắc. 

## Giải pháp Python 

Việc thực hiện dưới đây tuân theo mô phỏng kinh điển. Biểu diễn ngăn xếp sử dụng phần tử cuối cùng làm phần tử trên cùng, làm cho mỗi thao tác trở thành một lát danh sách, theo sau là một phần thêm vào. Các thủ tục phụ trợ kiểm tra tiền tố hiện tại và xây dựng phân tách khối an toàn duy nhất.```
Python
```Hai hàm trợ giúp mã hóa trực tiếp các hoạt động ngăn xếp.`move_dirty(k)`loại bỏ cái đầu tiên`k`các phần tử từ ngăn xếp bẩn và thêm chúng vào ngăn xếp trung gian.`move_middle(k)`thực hiện thao tác tương ứng từ ngăn xếp trung gian đến máy sấy. 

các`flush`thường lệ được gọi có chủ ý trước khi đưa ra quyết định cấu trúc khác. Điều này tương ứng với bằng chứng cho thấy việc bố trí hợp lệ hiện tại không bao giờ nên bị hoãn lại. Sự ràng buộc`k < b`được kiểm tra trong khi mở rộng một lần xả, do đó, một loại`2`hoạt động không bao giờ vượt quá khả năng của nó. 

Tiền tố gần như giảm dần được xây dựng lại từ ngăn xếp bẩn hiện tại. Bởi vì`n`chỉ là`2000`, việc quét tiền tố còn lại trên mỗi lần lặp sẽ đưa ra giới hạn bậc hai dự định và tránh các cấu trúc dữ liệu phức tạp. 

Mọi số học đều dựa trên số nguyên giới hạn bởi`n`, vì vậy Python không gặp vấn đề tràn. Điều kiện biên quan trọng là một thao tác phải di chuyển ít nhất một tấm, đó là lý do tại sao mọi số đếm được tạo ra đều hoàn toàn dương. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét```

```Mô phỏng kinh điển trước tiên xác định`[2,3]`như một khối liên tiếp. Nó phù hợp với một loại`1`hoạt động. Khối hữu ích tiếp theo là`[6]`, sau đó có thể được phơi ra và đặt vào máy sấy. 

| Bước | Ngăn xếp bẩn | Ngăn xếp trung gian | Tiến độ máy sấy | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 |`2 3 6 4 1 5`| trống | trống | bắt đầu | 
| 1 |`6 4 1 5`|`2 3`| trống |`1 2`| 
| 2 |`4 1 5`|`6 2 3`|`6`|`1 1`,`2 1`| 
| 3 |`5`|`4 1 2 3`|`6 5`|`1 2`,`1 1`,`2 1`| 
| 4 | trống |`1 2 3`|`6 5 4`|`2 1`,`2 3`| 

Thuộc tính quan trọng là mọi cấu hình trung gian vẫn không có khoảng trống ở các ranh giới quan trọng. Hai khối liên tiếp`[2,3]`Và`[4]`sau này có thể được phơi bày theo thứ tự yêu cầu. 

### Mẫu 2 

Hãy xem xét```

```Toàn bộ ngăn xếp bẩn là một khối liên tiếp và cả hai dung lượng đều đủ lớn. Một loại duy nhất`1`hoạt động chuyển tất cả các tấm, theo sau là một loại duy nhất`2`hoạt động. 

| Bước | Kích thước bẩn | Trung cấp | Máy sấy | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | 7 | trống | trống | bắt đầu | 
| 1 | 0 |`1 2 3 4 5 6 7`| trống |`1 7`| 
| 2 | 0 | trống |`1 2 3 4 5 6 7`|`2 7`| 

Ví dụ này thực hiện ranh giới công suất đầy đủ. Một triển khai sử dụng`< a`thay vì`<= a`, hoặc`< b`thay vì`<= b`, sẽ từ chối một giải pháp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^2)`| Tiền tố chuẩn và các khối của nó được quét lại nhiều nhất`O(n)`lặp lại cấu trúc. | 
| Không gian |`O(n)`| Ba ngăn xếp và các hoạt động được ghi lại chỉ chứa`O(n)`tấm và hoạt động. | 

Với`n <= 2000`,`O(n^2)`có nghĩa là nhiều nhất là vài triệu thao tác cơ bản trong quá trình triển khai dự định. Đó là mức thoải mái dưới mức mà cách tiếp cận bậc hai trở nên có vấn đề, trong khi việc tìm kiếm theo cấp số nhân trên các chuỗi phép toán là hoàn toàn không khả thi. 

## Trường hợp thử nghiệm```
Python
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1`|`YES`với các hoạt động hợp lệ | Ranh giới kích thước tối thiểu | 
|`5 5 5 / 1 2 3 4 5`|`YES`| Chuyển giao toàn bộ công suất | 
|`5 1 1 / 1 2 3 4 5`|`YES`| Mọi thao tác đều có kích thước chính xác là một | 
|`6 3 3 / 1 2 3 4 5 6`|`YES`| Dung lượng chính xác bằng kích thước khối | 

## Vỏ cạnh 

Đối với trường hợp tối thiểu```

```ngăn xếp bẩn chứa một đĩa. Trình tự hữu ích duy nhất có thể thực hiện được là di chuyển tấm đó qua ngăn xếp trung gian và vào máy sấy. Cả hai dung lượng đều chính xác bằng một, do đó thuật toán sử dụng các phép toán có kích thước một và kết thúc với ngăn xếp được sắp xếp hợp lệ duy nhất. 

Đối với trường hợp tất cả một công suất```

```không có hoạt động nào có thể di chuyển hai tấm. Do đó, thuật toán sẽ chia mỗi lần chuyển thành các đĩa riêng lẻ. Lý luận về cấu trúc vẫn được áp dụng vì một khối liên tiếp luôn có thể được chia thành các tấm đơn lẻ mà không tạo ra khoảng cách về số lượng giữa các giá trị lân cận. 

Đối với trường hợp đầy đủ công suất```

```toàn bộ hoán vị là một khối liên tiếp. Vì chiều dài của nó chính xác bằng cả hai công suất nên nó có thể được chuyển giao trong một thao tác ở mỗi giai đoạn. Điều này mắc phải một sai lầm phổ biến khi coi dung lượng là giới hạn trên nghiêm ngặt. 

Đối với mẫu thứ tư,```

```tiền tố an toàn tối đa cuối cùng chứa một cấu trúc không thể được chuyển trong khả năng sẵn có mà không tạo ra khoảng trống trong ngăn xếp trung gian. Khi thuật toán đạt đến cấu hình đó, không có vị trí ngay lập tức cũng như bất kỳ phân tách khối an toàn nào có sẵn, do đó nó báo cáo chính xác`NO`.
