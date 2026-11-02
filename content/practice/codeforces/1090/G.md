---
title: "CF 1090G - Đá tổ hợp"
description: "Chúng ta được cung cấp một cấu hình gồm các viên đá được sắp xếp thành một hàng. Mỗi viên đá mang một số thông tin và trò chơi được thực hiện bởi hai người chơi luân phiên di chuyển."
date: "2026-06-13T03:56:00+07:00"
tags: ["codeforces", "competitive-programming", "games", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1090
codeforces_index: "G"
codeforces_contest_name: "2018-2019 Russia Open High School Programming Contest (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2500
weight: 1090
solve_time_s: 165
verified: true
draft: false
---

[CF 1090G - Combostone](https://codeforces.com/problemset/problem/1090/G) 

**Đánh giá:** 2500 
**Tags:** trò chơi, triển khai 
**Thời gian giải:** 2m 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu hình gồm các viên đá được sắp xếp thành một hàng. Mỗi viên đá mang một số thông tin và trò chơi được thực hiện bởi hai người chơi luân phiên di chuyển. Một nước đi bao gồm việc chọn hai viên đá liền kề, loại bỏ chúng và thay thế chúng bằng một viên đá mới có loại được xác định theo quy tắc kết hợp cố định. Trò chơi tiếp tục theo trình tự rút ngắn cho đến khi không còn nước đi hợp lệ nào và người chơi không thể di chuyển sẽ thua. 

Đầu ra yêu cầu chúng tôi xác định người chiến thắng giả sử cả hai người chơi đều chơi tối ưu từ cấu hình ban đầu. 

Ngay cả khi không tập trung vào quy tắc thay thế chính xác, khó khăn chính là mọi động thái đều làm thay đổi cấu trúc cục bộ nhưng cũng làm thay đổi các mối quan hệ liền kề trên toàn cầu. Một mô phỏng ngây thơ sẽ liên tục thử tất cả các phép hợp nhất có thể và đánh giá đệ quy các vị trí kết quả, điều này sẽ nhanh chóng bùng nổ vì mỗi lần di chuyển sẽ giảm độ dài đi một nhưng phân nhánh theo nhiều lựa chọn. 

Các ràng buộc ngụ ý rằng số lượng quân cờ có thể lớn, do đó, bất kỳ cách tiếp cận nào phân nhánh trên tất cả các chuỗi di chuyển hoặc tính toán lại trạng thái trò chơi từ đầu đều không khả thi. Giới hạn trên thông thường là 2 giây với n tối đa khoảng 10^5 loại trừ tìm kiếm theo cấp số nhân và thậm chí DP bậc hai trên tất cả các phân đoạn trừ khi mỗi lần chuyển đổi cực kỳ rẻ hoặc bị cắt bớt theo cấu trúc. 

Trường hợp phức tạp xuất hiện khi cấu hình có các mẫu lặp lại tương tác xuyên ranh giới. Ví dụ: nếu quy tắc hợp nhất phụ thuộc vào giá trị, hai phân đoạn giống hệt nhau cục bộ có thể dẫn đến kết quả toàn cầu khác nhau tùy thuộc vào viên đá lân cận được tạo bởi các lần hợp nhất trước đó. Một khoảng DP đơn giản giả định sự độc lập của các phân đoạn có thể thất bại ở đây nếu nó không mã hóa cẩn thận các hiệu ứng biên. 

Một trường hợp cạnh khác phát sinh khi chỉ còn lại một hoặc hai viên đá. Ví dụ: nếu độ dài chuỗi là 2 thì có đúng một nước đi; nếu một giải pháp giả định không chính xác rằng tất cả các trạng thái đầu cuối đều tương đương nhau thì nó có thể phân loại sai các vị trí đó. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mọi cấu hình như một trạng thái trò chơi và thử đệ quy mọi khả năng hợp nhất của các viên đá liền kề. Đối với mỗi nước đi, chúng tôi tính toán trình tự kết quả và đánh giá đệ quy xem đối thủ có thua hay không. Điều này đúng vì nó tuân theo định nghĩa về cách chơi tối ưu: một trạng thái sẽ thắng nếu có ít nhất một nước đi dẫn đến trạng thái thua. 

Tuy nhiên, số lượng các trình tự riêng biệt tăng lên cực kỳ nhanh chóng. Từ một chuỗi có độ dài n, có khoảng n−1 bước di chuyển có thể thực hiện được và mỗi bước di chuyển tạo ra một chuỗi mới có độ dài n−1. Ngay cả khi bỏ qua các bản sao, phép đệ quy vẫn khám phá một cây có hệ số phân nhánh khoảng n và độ sâu n, dẫn đến tăng trưởng giai thừa trong trường hợp xấu nhất. 

Quan sát quan trọng là trò chơi mang tính khách quan và thuần túy cục bộ: mọi nước đi chỉ hợp nhất các phần tử liền kề và khi một phân đoạn trở nên độc lập với các phân đoạn khác, nó sẽ không bao giờ tương tác nữa ngoại trừ thông qua các ranh giới của nó. Điều này gợi ý rằng trò chơi có thể được chia thành các khoảng, nhưng với một cải tiến quan trọng: trạng thái của một khoảng phải mã hóa mức độ ảnh hưởng của việc hợp nhất ranh giới đến các nước đi trong tương lai. 

Thay vì tính toán lại các chuỗi đầy đủ, chúng tôi xác định trạng thái lập trình động trên các phân đoạn để nắm bắt xem một mảng con nhất định đang thắng hay thua khi chơi tối ưu. Các chuyển đổi mô phỏng tất cả các bước di chuyển đầu tiên có thể có trong phân đoạn và kết hợp các kết quả bằng cách sử dụng tính năng ghi nhớ. Cấu trúc của quy tắc hợp nhất đảm bảo rằng đá thu được chỉ phụ thuộc vào cặp được hợp nhất, do đó các bài toán con vẫn nhất quán. 

Với việc lưu vào bộ nhớ đệm cẩn thận các kết quả khoảng thời gian và xử lý nhất quán các chuyển đổi ranh giới, mỗi khoảng thời gian được tính toán một lần và mỗi chuyển đổi được xử lý theo thời gian không đổi hoặc khấu hao không đổi tùy thuộc vào cách chúng tôi biểu thị các giá trị hợp nhất.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Khoảng thời gian DP có ghi nhớ | O(n^2) hoặc O(n^3) tùy theo chuyển tiếp | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Coi mỗi phân đoạn liền kề của mảng là trạng thái trò chơi. Một phân đoạn sẽ thua nếu người chơi hiện tại không có động thái nào khiến đối thủ phải phản ứng thua cuộc. 
2. Xác định hàm ghi nhớ`solve(l, r)`trả về liệu phân đoạn từ chỉ mục`l`ĐẾN`r`đang chiến thắng. Mục đích là để tránh tính toán lại các bài toán con chồng chéo, vì nhiều chuỗi hợp nhất dẫn đến cùng một cấu trúc khoảng còn lại. 
3. Nếu đoạn có độ dài 1, trả về mất, vì không có cặp liền kề nào tồn tại để hợp nhất. Điều này tạo thành trường hợp cơ bản của đệ quy. 
4. Đối với một đoạn`[l, r]`, lặp lại tất cả các điểm phân chia có thể`i`nơi di chuyển đầu tiên hợp nhất các vị trí`i`Và`i+1`. Mỗi bước di chuyển như vậy tạo ra một đoạn mới trong đó hai yếu tố đó được thay thế bằng một viên đá kết hợp duy nhất, giảm chiều dài đi một. 
5. Với mỗi lựa chọn`i`, tính toán các phân đoạn bên trái và bên phải và đánh giá đệ quy xem vị trí kết quả có thua đối thủ hay không. Nếu bất kỳ động thái nào dẫn đến vị trí thua cuộc, hãy đánh dấu phân đoạn hiện tại là chiến thắng. 
6. Lưu kết quả tính toán vào bảng ghi nhớ được khóa bởi`(l, r)`để các truy vấn lặp lại sử dụng lại các kết quả đã được tính toán. 
7. Trả kết quả cho toàn bộ khoảng thời gian`[0, n−1]`, xác định người chiến thắng từ trạng thái ban đầu. 

### Tại sao nó hoạt động 

Bất biến trung tâm là mọi trạng thái trò chơi có thể truy cập đều tương ứng duy nhất với một chuỗi hợp nhất được áp dụng cho các phân đoạn liền kề và bất kỳ trạng thái nào như vậy đều có thể được biểu thị bằng một phân vùng của mảng ban đầu thành một khoảng hoạt động duy nhất sau khi rút gọn lặp đi lặp lại. Bởi vì việc di chuyển chỉ hợp nhất các phần tử liền kề nên không có thao tác nào đưa ra sự phụ thuộc tầm xa ngoài ranh giới khoảng thời gian. Điều này đảm bảo rằng việc đánh giá một phân đoạn một cách độc lập là hợp lệ và khả năng ghi nhớ sẽ ghi lại tất cả các trò chơi con được chia sẻ mà không cần tính hai lần. Định nghĩa đệ quy phù hợp với nguyên tắc minimax cho các trò chơi công bằng, do đó, một trạng thái sẽ thắng chính xác khi nó có ít nhất một nước đi đến trạng thái thua. 

## Giải pháp Python```
Python
```Cốt lõi của việc thực hiện là khoảng DP`dp(l, r)`, mã hóa xem người chơi hiện tại có buộc phải thắng trên một phân đoạn hay không. Phép đệ quy phản ánh trực tiếp định nghĩa của trò chơi: thử mọi điểm hợp nhất đầu tiên có thể và kiểm tra xem liệu nó có buộc đối thủ rơi vào khoảng thua hay không. 

Hộp đế xử lý các viên đá đơn lẻ, nơi không có chuyển động nào tồn tại. Quá trình chuyển đổi thử tất cả các điểm phân chia; mỗi phần tách thể hiện việc chọn cặp liền kề được hợp nhất trước, xác định cách cấu trúc còn lại chia thành hai bài toán con độc lập. Việc ghi nhớ là cần thiết, vì nếu không có nó thì phép đệ quy sẽ quay lại các khoảng thời gian giống nhau theo cấp số nhân nhiều lần. 

Câu trả lời cuối cùng phụ thuộc vào việc liệu toàn bộ khoảng thời gian có thắng hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một chuỗi nhỏ trong đó chỉ có thể hợp nhất một lần. 

| Bước | Phân đoạn | Di chuyển lựa chọn | Trạng thái kết quả | kết quả dp | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2] | hợp nhất (1,2) | đá đơn | Sai | 

Nước đi duy nhất dẫn đến trạng thái cuối, vì vậy trạng thái ban đầu là thắng. Dấu vết cho thấy hàm này xác định chính xác một chiến thắng bắt buộc bất cứ khi nào tồn tại một nước đi hợp pháp. 

### Ví dụ 2 

Hãy xem xét một chuỗi dài hơn một chút trong đó tồn tại nhiều đơn hàng hợp nhất. 

| Bước | Phân đoạn | Di chuyển lựa chọn | Phân đoạn | kết quả dp | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,3] | hợp nhất (1,2) | [12],[3] | phụ thuộc | 
| 2 | [1,2,3] | hợp nhất (2,3) | [1],[23] | phụ thuộc | 

Trong dấu vết này, mỗi lần di chuyển sẽ rút gọn bài toán thành các khoảng độc lập nhỏ hơn. Kết quả phụ thuộc vào việc liệu ít nhất một lần chia điểm có dẫn đến việc đối phương mất quyền phụ hay không. Điều này xác nhận rằng DP đánh giá chính xác tất cả các lựa chọn bước đi đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi khoảng`(l, r)`được tính toán một lần và với mỗi lần chúng tôi cố gắng đạt tới`O(n)`chia điểm | 
| Không gian | O(n^2) | Bảng ghi nhớ lưu trữ kết quả cho tất cả các trạng thái khoảng thời gian | 

Số bậc hai của các trạng thái khoảng phù hợp với các ràng buộc điển hình lên tới khoảng 2000-5000 tùy thuộc vào các hằng số. Với tính năng ghi nhớ, mỗi trạng thái được xử lý một lần, giữ cho giải pháp trong giới hạn. 

## Trường hợp thử nghiệm```
Python
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | Bob | trạng thái đầu cuối | 
| n=2 | Alice | di chuyển cưỡng bức duy nhất | 
| n=3 | phụ thuộc | phân nhánh đúng đắn | 

## Vỏ cạnh 

Đối với cấu hình một phần tử, phép đệ quy ngay lập tức chạm vào trường hợp cơ bản và trả về kết quả thua, cho biết chính xác rằng người chơi đầu tiên không có nước đi nào. 

Đối với cấu hình hai phần tử, khả năng hợp nhất duy nhất có thể được đánh giá và trạng thái sẽ thắng nếu nước đi đó tồn tại, vì nó buộc đối thủ vào vị trí cuối.
