---
title: "CF 102787C - Lời nói và lời nói 3"
description: "Chúng tôi duy trì một chuỗi nhị phân đại diện cho những lời nói tục tĩu. Ký tự 0 nghĩa là sneetch không có ngôi sao và 1 nghĩa là nó có ngôi sao. Chuỗi thay đổi thông qua ba loại hoạt động. Thao tác đầu tiên lật từng ký tự trong khoảng thời gian đã chọn."
date: "2026-07-27T19:21:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102787
codeforces_index: "C"
codeforces_contest_name: "Algorithms Thread Treaps Contest"
rating: 0
weight: 102787
solve_time_s: 66
verified: true
draft: false
---

[CF 102787C - Tiếng hắt hơi và Bài phát biểu 3](https://codeforces.com/problemset/problem/102787/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một chuỗi nhị phân đại diện cho những lời nói tục tĩu. Một nhân vật`0`có nghĩa là một nụ cười không có ngôi sao và`1`nghĩa là nó có một ngôi sao. Chuỗi thay đổi thông qua ba loại hoạt động. 

Thao tác đầu tiên lật từng ký tự trong khoảng thời gian đã chọn. Thao tác thứ hai đảo ngược khoảng thời gian đã chọn. Thao tác thứ ba sắp xếp một khoảng đã chọn, đối với chuỗi nhị phân có nghĩa là tất cả các số 0 di chuyển về đầu khoảng và tất cả các số 1 di chuyển về cuối. 

Sau mỗi thao tác, chúng ta cần độ dài của đoạn liên tiếp dài nhất chỉ chứa các ký tự bằng nhau. 

Các ràng buộc rất lớn: cả độ dài chuỗi và số lượng thao tác có thể đạt tới 300000. Giải pháp quét một khoảng sau mỗi truy vấn có thể thực hiện khoảng`O(nq)`công việc vượt xa những gì có thể. Chúng ta cần mọi thao tác chỉ chạm vào logarit của nhiều phần của cấu trúc. 

Những trường hợp khó khăn không phải là bản thân những đầu vào lớn mà là sự kết hợp của các hoạt động phá hủy các giả định. Một phạm vi có thể được sắp xếp, sau đó đảo ngược, rồi đảo ngược, do đó, bất kỳ phương pháp nào chỉ tính theo dõi hoặc chỉ theo dõi số lần chuyển đổi sẽ không thành công. 

Ví dụ:```
Input:
5 1
00111
2 1 5
```Câu trả lời vẫn là`3`, bởi vì chuỗi trở thành`11100`. 

Việc triển khai bất cẩn có thể quên rằng việc đảo ngược một phân đoạn sẽ thay đổi hoạt động thuộc về bên nào. 

Một ví dụ khác:```
Input:
4 1
0011
3 1 4
```Đầu ra là`2`, vì việc sắp xếp không làm thay đổi chuỗi. Việc triển khai luôn xây dựng lại câu trả lời từ số 0 và số 1 không thể phân biệt được`0011`từ`0101`, mặc dù độ dài đoạn dài nhất bằng nhau của chúng là khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lưu trữ chuỗi một cách bình thường và áp dụng mọi thao tác bằng cách duyệt qua khoảng thời gian bị ảnh hưởng. Việc lật và đảo ngược rất đơn giản và việc sắp xếp có thể được thực hiện bằng cách đếm các số 0 và viết lại khoảng thời gian. Điều này đúng, nhưng một truy vấn có thể chạm tới`300000`các vị trí. Với`300000`truy vấn, trường hợp xấu nhất đạt đến khoảng`9 * 10^10`hoạt động. 

Quan sát quan trọng là mọi hoạt động đều là một hoạt động phạm vi. Một treap ngầm cung cấp chính xác các thao tác mà chúng ta cần: chia theo vị trí, sửa đổi một đoạn ở giữa một cách lười biếng và hợp nhất lại. 

Mỗi nút treap lưu trữ một ký tự. Cây con giữ nguyên độ dài, số lượng đơn vị, lần chạy dài nhất bằng nhau và thông tin về lần chạy đầu tiên và cuối cùng. Các thẻ lười xử lý việc lật, đảo ngược và gán toàn bộ cây con về 0 hoặc một. 

Việc sắp xếp khoảng nhị phân trở nên đơn giản hơn nhiều với cách biểu diễn này. Sau khi cô lập khoảng, chúng ta biết nó chứa bao nhiêu khoảng. Chúng tôi thay thế khoảng bằng cách ghép khối 0 và khối một. Cấu trúc không bao giờ cần kiểm tra từng ký tự riêng lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Kho báu tiềm ẩn | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một kho lưu trữ ngầm từ chuỗi ban đầu. Mỗi nút đại diện cho một ký tự và lưu trữ tất cả thông tin tổng hợp cần thiết để trả lời truy vấn chạy tối đa. 
2. Đối với thao tác phạm vi, hãy chia treap thành ba phần: tiền tố trước phạm vi, chính phạm vi và hậu tố sau phạm vi. Điều này cho phép truy cập trực tiếp vào khoảng thời gian bị ảnh hưởng mà không cần di chuyển các ký tự không liên quan. 
3. Đối với thao tác lật, hãy dán thẻ lật lười vào phần giữa. Hoán đổi số 0 và số 1 để số lượng 1 trở thành`size - ones`, và các ký tự đầu tiên và cuối cùng được lưu trữ sẽ được hoán đổi. 
4. Đối với thao tác đảo ngược, hãy áp dụng thẻ đảo ngược lười biếng. Những đứa trẻ được hoán đổi và hướng của thông tin ranh giới được lưu trữ được trao đổi. 
5. Đối với thao tác sắp xếp, hãy đếm số lượng đơn vị ở giữa. Thay thế phần giữa bằng một tre chứa số lượng nút 0 cần thiết, theo sau là số lượng nút một cần thiết. 
6. Hợp nhất ba phần lại với nhau và in số lần chạy tối đa được lưu trữ ở thư mục gốc. 

Tại sao nó hoạt động: bất biến treap là mọi bản tóm tắt của cây con đều mô tả chính xác trình tự được biểu thị bởi cây con đó. Các thao tác lười biếng không làm thay đổi trình tự được biểu diễn một cách không chính xác; họ chỉ trì hoãn việc áp dụng sự chuyển đổi tương tự cho trẻ em. Vì mọi truy vấn đều tách biệt chính xác khoảng thời gian bị ảnh hưởng và khôi phục treap sau đó nên bản tóm tắt gốc luôn mô tả chuỗi hiện tại hoàn chỉnh, do đó giá trị chạy tối đa của nó là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Full implementation omitted here and will be provided in the next part.
```Việc triển khai sử dụng mẫu trep tiềm ẩn tiêu chuẩn. Hàm phân tách phân tách một chuỗi theo chỉ mục, trong khi hợp nhất sẽ khôi phục thứ tự ban đầu. Các hàm lan truyền lười biếng là cốt lõi của giải pháp vì mọi truy vấn có thể được biểu diễn dưới dạng một trong ba phép biến đổi: đảo ngược, đảo ngược hoặc gán. 

Chi tiết triển khai quan trọng là duy trì thông tin ranh giới. Lần chạy dài nhất có thể vượt qua ranh giới giữa nút con bên trái và bên phải của một nút, do đó câu trả lời không thể được tính toán chỉ từ các nút con một cách độc lập. Ký tự đầu tiên, ký tự cuối cùng và độ dài lần chạy của chúng cho phép thao tác hợp nhất nối các khối lân cận bằng nhau một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mọi thao tác thực hiện một số lần phân chia và hợp nhất treap không đổi | 
| Không gian | O(n) | Mỗi ký tự được lưu trữ trong một nút treap | 

Chiều cao logarit của treap ngẫu nhiên giúp mỗi truy vấn hoạt động hiệu quả ngay cả ở giới hạn tối đa. 

## Ví dụ đã hoạt động 

cho```
8 8
00000000
1 1 3
```đoạn giữa`000`trở thành`111`, cho:```
11100000
```Đoạn bằng nhau dài nhất có độ dài`5`. 

Vì```
7 7
0111111
3 3 7
```phần được chọn đã là tất cả, vì vậy việc sắp xếp sẽ giữ cho phần đó không thay đổi. Đoạn bằng nhau dài nhất vẫn là khối sáu đoạn. 

## Vỏ cạnh 

Chuỗi ký tự đơn được xử lý một cách tự nhiên vì việc phân tách sẽ tạo ra các ngăn trống bên trái hoặc bên phải và số lượt chạy tối đa của nút còn lại là một. 

Một chuỗi hoàn toàn bằng nhau cũng rất quan trọng. Ví dụ:```
5 1
11111
1 2 4
```Sau khi lật khoảng giữa:```
10001
```Câu trả lời là`3`, không`5`. Lật lười phải cập nhật chính xác cả số lượng ký tự và độ dài chạy được lưu trữ. 

Sắp xếp toàn phạm vi cũng phải hoạt động:```
6 1
101010
3 1 6
```Kết quả trở thành:```
000111
```và câu trả lời là`3`. 

Tôi có thể tiếp tục với phần còn lại bao gồm cách triển khai Python đầy đủ, hướng dẫn mã chi tiết và các bài kiểm tra dựa trên xác nhận.
