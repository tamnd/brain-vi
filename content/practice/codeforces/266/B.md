---
title: "CF 266B - Xếp hàng tại trường"
description: "Chúng ta có một dòng trẻ em được biểu diễn dưới dạng một chuỗi trong đó mỗi nhân vật là con trai hoặc con gái. Dòng phát triển theo thời gian theo các bước riêng biệt. Trong mỗi giây, mỗi cặp liền kề mà một chàng trai đứng ngay trước một cô gái sẽ đổi vị trí đồng thời."
date: "2026-06-04T18:12:53+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graph-matchings", "implementation", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 266
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 163 (Div. 2)"
rating: 800
weight: 266
solve_time_s: 72
verified: true
draft: false
---

[CF 266B - Xếp hàng tại trường](https://codeforces.com/problemset/problem/266/B) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, so khớp biểu đồ, triển khai, đường dẫn ngắn nhất 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dòng trẻ em được biểu diễn dưới dạng một chuỗi trong đó mỗi nhân vật là con trai hoặc con gái. Dòng phát triển theo thời gian theo các bước riêng biệt. Trong mỗi giây, mỗi cặp liền kề mà một chàng trai đứng ngay trước một cô gái sẽ đổi vị trí đồng thời. Nói cách khác, tất cả các mẫu “BG” hợp lệ sẽ trở thành “GB” trong cùng một giây và tất cả các giao dịch hoán đổi diễn ra song song chứ không phải lần lượt từ trái sang phải. 

Nhiệm vụ là xác định cấu hình của đường dây sau đúng t giây áp dụng quy tắc chuyển đổi này. 

Các ràng buộc rất nhỏ, với cả n và t nhiều nhất là 50. Điều này ngay lập tức cho chúng ta biết rằng ngay cả một mô phỏng đơn giản cũng có thể chấp nhận được vì số thao tác trong trường hợp xấu nhất là theo thứ tự n nhân với t, nhiều nhất là 2500 lượt kiểm tra hoán đổi mỗi lần chạy. Ngay cả khi chúng tôi lặp lại quá trình quét ký tự trong mỗi giây, tổng công việc vẫn không đáng kể trong giới hạn 2 giây. 

Một trường hợp phức tạp phát sinh từ tính chất đồng thời của các giao dịch hoán đổi. Nếu chúng ta xử lý từ trái sang phải và ngay lập tức hoán đổi các ký tự tại chỗ, chúng ta có thể vô tình sử dụng lại thông tin cập nhật trong cùng một giây. Ví dụ: hãy xem xét “BGBG”. Nếu chúng ta tham lam hoán đổi cặp đầu tiên “BG”, chúng ta sẽ nhận được “GBBG”, và sau đó cặp tiếp theo được đánh giá không chính xác vì cấu trúc đã thay đổi ở giữa bước. Cách giải thích đúng yêu cầu tất cả các lần hoán đổi trong một giây phải dựa trên cùng một ảnh chụp nhanh của chuỗi. 

Một trường hợp khác là khi các giao dịch hoán đổi chồng chéo lên nhau. Trong mẫu như “BGB”, chỉ cặp đầu tiên hoán đổi trong một bước duy nhất và nhân vật ở giữa tham gia tối đa một lần hoán đổi mỗi giây. Điều này ngăn chặn chuyển động kép trong một đơn vị thời gian. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để mô hình hóa quy trình là mô phỏng từng giây theo đúng nghĩa đen. Đối với mỗi bước thời gian, chúng tôi quét chuỗi từ trái sang phải và bất cứ khi nào chúng tôi nhìn thấy mẫu “BG”, chúng tôi sẽ hoán đổi nó thành “GB” và bỏ qua vị trí tiếp theo vì nó đã di chuyển như một phần của hoán đổi. Cách tiếp cận này khớp chính xác với định nghĩa vấn đề và duy trì tính chính xác vì chúng tôi mô phỏng từng giây đầy đủ bằng cách sử dụng một lần chuyển mới qua trạng thái hiện tại. 

Mô phỏng lực lượng vũ phu này đã hoạt động đủ tốt vì mỗi giây yêu cầu một lần truyền qua chuỗi, mang lại công việc O(n) mỗi giây. Trong t giây, tổng công việc là O(n·t), được giới hạn bởi 2500 thao tác. 

Không cần phải tối ưu hóa nâng cao hơn vì các ràng buộc không đẩy chúng ta tới những cải tiến tiệm cận. Thông tin chi tiết quan trọng không phải là về việc tăng tốc tính toán mà là về việc lập mô hình chính xác các giao dịch hoán đổi đồng thời bằng cách đảm bảo chúng tôi không xâu chuỗi các cập nhật trong vòng một giây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng từng bước | O(n·t) | O(n) | Đã chấp nhận | 
| Mô hình chuyển động toàn cầu được tối ưu hóa | O(n) | O(n) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Bắt đầu với hàng đợi ban đầu dưới dạng danh sách các ký tự có thể thay đổi. Điều này cho phép trao đổi hiệu quả. 
2. Lặp lại quá trình này trong t giây. Mỗi lần lặp đại diện cho một bước chuyển đổi được đồng bộ hóa đầy đủ. 
3. Trong mỗi giây, quét từ trái sang phải bằng chỉ số i bắt đầu từ 0. 
4. Nếu vị trí hiện tại i và vị trí tiếp theo i+1 tạo thành mẫu “B” theo sau là “G”, hãy hoán đổi chúng. 
5. Sau khi thực hiện hoán đổi, tăng i lên 2 để tránh xử lý lại ký tự mới được hoán đổi. 
6. Nếu không có sự hoán đổi nào xảy ra ở vị trí i, hãy di chuyển i về phía trước 1. 
7. Sau khi hoàn tất quá trình quét toàn bộ, danh sách kết quả sẽ thể hiện hàng đợi sau một giây. 
8. Tiếp tục cho đến khi tất cả t giây được mô phỏng.

Lý do chính đằng sau việc bỏ qua các chỉ số sau khi hoán đổi là cặp “GB” đã hoán đổi không nên được xem xét lại trong cùng một giây. Nếu chúng ta không bỏ qua, cùng một cô gái có thể tham gia vào nhiều lần hoán đổi trong một bước thời gian, điều này vi phạm quy tắc hoán đổi đồng thời. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên tính bất biến mà ở đầu mỗi giây, mảng biểu thị trạng thái sau chính xác nhiều phép biến đổi đầy đủ đó. Trong vòng một giây, mỗi lần hoán đổi chỉ được áp dụng dựa trên cấu hình ban đầu của giây đó vì chúng tôi quét từ trái sang phải và đảm bảo rằng khi hoán đổi xảy ra, cả hai vị trí đều được sử dụng cho bước đó. Điều này ngăn chặn các hiệu ứng xếp tầng trong cùng một giây và đảm bảo tất cả các giao dịch hoán đổi đều tương ứng chính xác với các cặp “BG” rời rạc trong ảnh chụp nhanh hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, t = map(int, input().split())
s = list(input().strip())

for _ in range(t):
    i = 0
    while i < n - 1:
        if s[i] == 'B' and s[i + 1] == 'G':
            s[i], s[i + 1] = s[i + 1], s[i]
            i += 2
        else:
            i += 1

print("".join(s))
```Giải pháp lưu trữ hàng đợi dưới dạng danh sách nên các giao dịch hoán đổi là O(1). Mỗi giây được mô phỏng độc lập. Vòng lặp bên trong thực thi cẩn thận quy tắc hoán đổi dựa trên một cấu hình cố định duy nhất mỗi giây bằng cách quét một lần và nâng cấp chỉ mục một cách thích hợp. 

Một lỗi phổ biến là cập nhật chuỗi từ trái sang phải mà không bỏ qua các chỉ mục sau khi hoán đổi. Điều đó dẫn đến việc xử lý kép một ký tự vừa di chuyển, vi phạm tính chất đồng thời của việc chuyển đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1
BGGBG
```Chúng tôi mô phỏng một giây. 

| tôi | kiểm tra cặp | hành động | tiểu bang | 
| --- | --- | --- | --- | 
| 0 | BG | trao đổi | GBGBG | 
| 2 | GB | không | GBGBG | 
| 3 | BG | trao đổi | GBGGB | 

Kết quả cuối cùng là`GBGGB`. 

Điều này cho thấy các giao dịch hoán đổi là độc lập và chỉ dựa trên cấu hình ban đầu của giây đó. 

### Ví dụ 2 

đầu vào:```
4 2
BGBG
```Chúng tôi theo dõi hai giây. 

Sau giây 1: 

| tôi | cặp | hành động | tiểu bang | 
| --- | --- | --- | --- | 
| 0 | BG | trao đổi | GBGB | 
| 2 | BG | trao đổi | GGBB | 

Sau giây thứ 2: 

| tôi | cặp | hành động | tiểu bang | 
| --- | --- | --- | --- | 
| 0 | GG | không | GGBB | 
| 1 | GB | không | GGBB | 
| 2 | BB | không | GGBB | 

Kết quả cuối cùng là`GGBB`. 

Điều này chứng tỏ sự chuyển động đã bão hòa một cách tự nhiên như thế nào khi tất cả các bé gái đều đã đi trước các bé trai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·t) | Mỗi t giây thực hiện một lần quét tuyến tính trên chuỗi | 
| Không gian | O(n) | Chúng tôi lưu trữ hàng đợi dưới dạng danh sách các ký tự có thể thay đổi | 

Cho n, t ≤ 50, số lượng thao tác tối đa là rất nhỏ, thấp hơn nhiều so với bất kỳ giới hạn thực tế nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, t = map(int, input().split())
    s = list(input().strip())

    for _ in range(t):
        i = 0
        while i < n - 1:
            if s[i] == 'B' and s[i + 1] == 'G':
                s[i], s[i + 1] = s[i + 1], s[i]
                i += 2
            else:
                i += 1

    return "".join(s)

# provided sample
assert run("5 1\nBGGBG\n") == "GBGGB"

# all same
assert run("4 3\nGGGG\n") == "GGGG"

# all boys
assert run("4 2\nBBBB\n") == "BBBB"

# alternating pattern
assert run("6 1\nBGBGBG\n") == "GBGBGB"

# multiple steps
assert run("5 2\nBGGBG\n") == run("5 1\n" + "GBGGB\n")

# max small stress
assert run("1 50\nB\n") == "B"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 3/GGGG | GGGG | Không có giao dịch hoán đổi xảy ra | 
| 4 2/BBBB | BBBB | Không có cặp BG hợp lệ | 
| 6 1/BGBGBG | GBGBGB | Nhiều giao dịch hoán đổi độc lập | 
| 1 50/B | B | Kích thước ranh giới tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các giao dịch hoán đổi liền kề nhau, chẳng hạn như “BGB”. Trong một giây, chỉ “BG” đầu tiên mới được hoán đổi, tạo ra “GBB”. Thuật toán xử lý việc này một cách chính xác vì sau khi hoán đổi chỉ số 0 và 1, con trỏ sẽ nhảy đến chỉ mục 2, ngăn ký tự thứ hai tham gia vào một hoán đổi khác trong cùng một giây. 

Một trường hợp cạnh khác là một chuỗi xen kẽ hoàn toàn như “BGBGBG”. Thuật toán đảm bảo rằng các giao dịch hoán đổi diễn ra độc lập đối với từng cặp rời rạc và việc bỏ qua sau khi hoán đổi sẽ ngăn ngừa tình trạng hỏng chồng chéo. 

Cuối cùng, trường hợp no-op chẳng hạn như tất cả các chuỗi “G” hoặc tất cả các chuỗi “B” xác nhận tính ổn định. Vì không tồn tại mẫu “BG” nên quá trình quét không thực hiện hoán đổi trong bất kỳ giây nào và trạng thái bất biến được giữ nguyên trong tất cả các lần lặp.
