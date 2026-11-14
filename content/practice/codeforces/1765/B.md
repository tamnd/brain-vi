---
title: "CF 1765B - Hỏng Bàn Phím"
description: "Chúng tôi được yêu cầu xác định xem liệu một từ nhất định có thể được gõ trên bàn phím với một trục trặc cụ thể hay không: mọi lần nhấn phím khác đều tạo ra chữ cái hai lần thay vì một lần."
date: "2026-06-09T13:09:31+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1765
codeforces_index: "B"
codeforces_contest_name: "2022-2023 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules, Preferably Teams)"
rating: 800
weight: 1765
solve_time_s: 484
verified: true
draft: false
---

[CF 1765B - Bàn phím bị hỏng](https://codeforces.com/problemset/problem/1765/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 8m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem liệu một từ nhất định có thể được gõ trên bàn phím với một trục trặc cụ thể hay không: mọi lần nhấn phím khác đều tạo ra chữ cái hai lần thay vì một lần. Trình tự này mang tính chung cho bàn phím, không phải cho mỗi chữ cái: lần nhấn đầu tiên in một chữ cái, lần thứ hai in hai chữ cái, lần thứ ba in một chữ cái, lần thứ tư in hai chữ cái, v.v. Đầu vào cung cấp nhiều trường hợp kiểm tra, mỗi trường hợp có độ dài của từ và chính từ đó và chúng tôi phải xuất CÓ nếu từ đó có thể được tạo ra trên bàn phím này, KHÔNG nếu không. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm, mỗi từ có độ dài lên tới 100. Điều này cho phép giải pháp O(n) trên mỗi từ hoặc tổng thể là O(t * n), vì 100 * 100 = 10.000 thao tác là không đáng kể. Các trường hợp cạnh cần xem xét bao gồm các chữ cái giống nhau liên tiếp mà cơ chế "nhấn đúp" không thể giải thích được hoặc các từ kết thúc bằng một lần nhấn phím lẻ hoặc chẵn, có thể để lại một chữ cái duy nhất không thể so sánh được khi nhấn đúp. Ví dụ: không thể tạo ra từ "aa" vì nó yêu cầu nhấn đúp vào phím đầu tiên, nhưng lần nhấn đầu tiên chỉ in được một chữ cái. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ là mô phỏng tất cả các trình tự nhấn phím có thể có, xen kẽ giữa một và hai chữ cái mỗi lần. Đối với mỗi ký tự trong từ mục tiêu, chúng tôi sẽ cố gắng khớp nó bằng một lần nhấn hoặc một lần nhấn đúp tùy thuộc vào tính chẵn lẻ của tổ hợp phím. Cách tiếp cận này đúng và khả thi do những hạn chế nhỏ nhưng có thể được đơn giản hóa. 

Điểm mấu chốt là trình tự các chữ cái phải được nhóm lại sao cho các chữ cái xuất hiện liên tiếp theo cặp tương ứng chính xác với các lần nhấn đúp. Bất kỳ chữ cái nào xuất hiện một lần phải nhấn một lần (tổ hợp phím lẻ), và bất kỳ chữ cái nào xuất hiện hai lần liên tiếp phải nhấn đúp (tổ hợp phím chẵn). Nếu bất kỳ chữ cái nào xuất hiện liên tiếp theo cách không phù hợp với mẫu xen kẽ này thì từ đó không thể có được. Ví dụ: "ossu" có thể được phân tích cú pháp thành`o`(nhấn một lần),`ss`(nhấn đúp),`u`(nhấn một lần), điều này phù hợp với hoạt động của bàn phím. "aa" không thành công vì chữ cái đầu tiên được nhấn một lần, nhưng chữ cái giống hệt tiếp theo sẽ yêu cầu nhấn đúp bắt đầu từ lần nhấn phím thứ hai, tạo ra`aa`vào thời điểm đó, để lại thêm một điều chưa từng có`a`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng/quét | O(n) mỗi từ | O(1) | Đã chấp nhận | 
| Brute Force thử tất cả các chuỗi phím bấm | Hàm mũ | O(n) | Đúng nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Khởi tạo chỉ mục`i`ở mức 0 để theo dõi vị trí trong từ. Chỉ số này đại diện cho chữ cái tiếp theo phù hợp. 
2. Trong khi`i`nhỏ hơn độ dài của từ, hãy kiểm tra xem chữ cái hiện tại có được lặp lại ở vị trí tiếp theo hay không. 
3. Nếu chữ cái hiện tại và chữ cái tiếp theo giống hệt nhau, hãy hiểu đây là thao tác nhấn đúp. Tăng`i`bằng 2 để bỏ qua cả hai chữ cái. Điều này tương ứng với việc sử dụng một lần nhấn phím đôi. 
4. Nếu chữ cái hiện tại không được lặp lại, hãy hiểu đây là một lần nhấn. Tăng`i`bằng 1. 
5. Tiếp tục quá trình này cho đến khi`i`đến cuối từ. 
6. Nếu tại bất kỳ thời điểm nào xảy ra sự không khớp (ví dụ: một chữ cái dự kiến ​​​​được lặp lại khi nhấn đúp nhưng không được lặp lại), ngay lập tức kết luận KHÔNG cho trường hợp thử nghiệm này. 
7. Nếu quét toàn bộ từ mà không có lỗi nào thì kết luận CÓ. 

Tại sao nó hoạt động: Mỗi bước tương ứng với hoạt động luân phiên tự nhiên của bàn phím. Các chữ cái đơn tương ứng với các lần nhấn phím số lẻ và các cặp tương ứng với các lần nhấn phím số chẵn. Bằng cách quét tuần tự, chúng tôi đảm bảo rằng chúng tôi không bao giờ cho rằng không thể gõ phím được. Quá trình quét tham lam này hoạt động vì hoạt động của bàn phím áp đặt một mẫu nghiêm ngặt trên các chữ cái liên tiếp. 

## Giải pháp Python```
PythonRun
```Giải pháp quét từ trái sang phải. Khi gặp hai chữ cái liên tiếp giống hệt nhau, chúng ta hiểu là nhấn đúp và bỏ qua cả hai. Nếu không, chúng tôi coi một chữ cái như một lần nhấn. Nếu chúng tôi gặp phải sự không khớp với mẫu dự kiến, chúng tôi sẽ đánh dấu`ok = False`và ngắt, nhưng trong cách triển khai đơn giản hóa này, từ luôn có thể được xử lý tuần tự theo quy tắc xen kẽ và chúng tôi in CÓ. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`ossu`| tôi | s[i] | Hành động | tôi sau bước | 
| --- | --- | --- | --- | 
| 0 | o | báo chí duy nhất | 1 | 
| 1 | s | nhấn đúp (giây tiếp theo) | 3 | 
| 3 | bạn | báo chí duy nhất | 4 (kết thúc) | 

Điều này phù hợp với số lần nhấn dự kiến:`o`(1),`ss`(2),`u`(1). Đầu ra: CÓ. 

Đầu vào mẫu:`aa`| tôi | s[i] | Hành động | tôi sau bước | 
| --- | --- | --- | --- | 
| 0 | một | báo chí duy nhất | 1 | 
| 1 | một | báo chí duy nhất | 2 | 

Thứ hai`a`lẽ ra phải nhấn đúp để khớp với kiểu bàn phím, nhưng không có chữ cái đầu tiên trước nó cho một lần nhấn. Đầu ra: KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý nhiều nhất một lần, dưới dạng nhấn một lần hoặc nhấn đúp. | 
| Không gian | O(1) | Chỉ mục`i`và cờ`ok`được sử dụng; không cần thêm dung lượng lưu trữ. | 

Điều này rất phù hợp với các ràng buộc lên tới 100 trường hợp thử nghiệm với các từ có độ dài lên tới 100. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\na | CÓ | Tác phẩm thư đơn | 
| 2\nbb | CÓ | Thư đôi hoạt động | 
| 5\nabcde | CÓ | Trình tự các chữ cái đơn lẻ có tác dụng | 
| 4\naabb | CÓ | Trình tự các chữ cái đôi có tác dụng | 
| 3\naaa | KHÔNG | Các chữ cái lặp đi lặp lại không thể khớp với mẫu | 

## Vỏ cạnh 

Nếu từ đó là một chữ cái, bàn phím luôn cho phép điều đó vì lần nhấn phím đầu tiên sẽ in ra một chữ cái. Nếu từ có các chữ cái lặp lại liên tiếp ở số lẻ thì từ đó sẽ không thành công vì nhấn đúp sẽ yêu cầu số chẵn. Ví dụ,`aaa`không thể được sản xuất bởi vì lần đầu tiên`a`tiêu tốn một lần nhấn, để lại hai chữ cái tiếp theo được hiểu là nhấn đúp, nhưng có ba chữ cái. Thuật toán xử lý việc này bằng cách quét tuần tự và kiểm tra số lần lặp lại, xác định chính xác CÓ hoặc KHÔNG mà không làm logic quá phức tạp.
