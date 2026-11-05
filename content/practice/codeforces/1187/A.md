---
title: "CF 1187A - Nhãn dán và Đồ chơi"
description: "Mỗi truy vấn mô tả một tập hợp các quả trứng giống hệt nhau. Mỗi quả trứng đều có sự kết hợp của hai vật phẩm có thể có: nhãn dán và đồ chơi. Trên tất cả các quả trứng, có tổng cộng chính xác s nhãn dán và chính xác t đồ chơi và mỗi quả trứng đóng góp một hoặc cả hai vật phẩm này."
date: "2026-06-13T12:31:27+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1187
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 67 (Rated for Div. 2)"
rating: 900
weight: 1187
solve_time_s: 254
verified: true
draft: false
---

[CF 1187A - Hình dán và Đồ chơi](https://codeforces.com/problemset/problem/1187/A) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 4 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi truy vấn mô tả một tập hợp các quả trứng giống hệt nhau. Mỗi quả trứng đều có sự kết hợp của hai vật phẩm có thể có: nhãn dán và đồ chơi. Trên tất cả các quả trứng, có chính xác`s`dán và chính xác`t`tổng số đồ chơi và mỗi quả trứng đóng góp một hoặc cả hai món này. 

Chúng tôi không biết các vật phẩm được phân bổ như thế nào giữa các quả trứng, chỉ có tổng số toàn cầu là cố định. Khi chúng ta mua trứng, chúng ta đang chọn một tập hợp con tùy ý một cách hiệu quả và kẻ thù có thể giả định cách sắp xếp tồi tệ nhất có thể là trứng chỉ có nhãn dán, trứng chỉ có đồ chơi và trứng hỗn hợp. 

Nhiệm vụ là xác định số lượng trứng nhỏ nhất mà chúng ta phải mua sao cho dù sắp xếp phân phối như thế nào, chúng ta vẫn đảm bảo nhận được ít nhất một nhãn dán và ít nhất một món đồ chơi. 

Khó khăn chính là việc mua trứng không tiết lộ nội dung bên trong. Chúng tôi đang giải quyết vấn đề đảm bảo tổ hợp trong trường hợp xấu nhất chứ không phải vấn đề lấy mẫu mang tính xây dựng. 

Các ràng buộc cho phép tối đa 100 truy vấn với các giá trị là`n, s, t`lên đến 10^9. Điều này ngay lập tức loại trừ mọi mô phỏng hoặc liệt kê trên các tập hợp con trứng. Giải pháp phải giảm mỗi truy vấn thành số học có thời gian không đổi. 

Một cách giải thích ngây thơ có thể gợi ý việc cố gắng suy luận về tất cả các phân bố có thể có hoặc kiểm tra các tiền tố của một thứ tự tưởng tượng. Điều đó không thành công vì đối thủ luôn có thể sắp xếp lại các vật phẩm để tối đa hóa số lượng trứng bạn cần trước khi nhìn thấy cả hai loại vật phẩm. 

Một vài trường hợp đặc biệt làm rõ cấu trúc. 

Nếu tất cả trứng chứa cả hai mục, chẳng hạn như`n = 10, s = 10, t = 10`, thì một quả trứng đã đảm bảo cả nhãn dán và đồ chơi. Bất kỳ câu trả lời nào lớn hơn sẽ không chính xác vì sự đảm bảo là ngay lập tức. 

Ví dụ: nếu nhãn dán và đồ chơi được tách biệt hoàn hảo`n = 2, s = 1, t = 1`, thì mỗi quả trứng chứa đúng một vật phẩm và chúng ta phải mua cả hai quả trứng trong trường hợp xấu nhất. 

Tình huống khó nhận thấy nhất là khi một loại bị khan hiếm trong phân phối. Ví dụ`n = 10, s = 5, t = 7`ngụ ý sự chồng chéo tồn tại bởi vì`s + t > n`, nhưng chúng ta không thể giả định mức độ chồng chéo hoặc tách biệt tồn tại cục bộ trong một tập hợp con trứng. 

## Phương pháp tiếp cận 

Chiến lược vũ phu sẽ cố gắng mô hình hóa mọi cách sắp xếp trứng có thể phù hợp với các ràng buộc. Đối với mỗi cách sắp xếp có thể, chúng tôi sẽ mô phỏng việc mua từng quả trứng một và kiểm tra khi cả nhãn dán và đồ chơi xuất hiện. Điều này là không khả thi vì số lượng sắp xếp hợp lệ tăng theo cấp số nhân với`n`, và thậm chí việc đại diện cho chúng là không thể dưới những ràng buộc. 

Một quan điểm hữu ích hơn là lý luận theo hướng tránh trường hợp xấu nhất. Chúng tôi muốn hiểu làm thế nào kẻ thù có thể trì hoãn thời điểm chúng tôi có được cả nhãn dán và đồ chơi. Mục tiêu của kẻ thù là tập trung càng nhiều càng tốt một trong hai vật phẩm vào những quả trứng mà chúng ta có thể nhặt sớm. 

Trường hợp xấu nhất xảy ra khi chúng ta buộc phải chọn những quả trứng có chứa một loại duy nhất. Yếu tố giới hạn trở thành số lượng trứng tối đa có thể tránh cho chúng ta một trong hai vật phẩm. 

Nếu chúng ta nghĩ đến việc tránh hoàn toàn các nhãn dán, điều tốt nhất mà kẻ thù có thể làm là đặt tất cả các nhãn dán vào càng ít quả trứng càng tốt, để lại nhiều quả trứng chỉ dành cho đồ chơi. Tương tự, để tránh đồ chơi, bé có thể tập trung đồ chơi. 

Quan sát quan trọng là nút cổ chai được xác định bởi mức độ chồng chéo tồn tại giữa việc phân phối nhãn dán và đồ chơi. Nếu nhiều trứng chứa cả hai thì vấn đề trở nên dễ dàng. Nếu có ít hoặc không có sự trùng lặp nào, chúng tôi buộc phải lấy mẫu cả hai loại riêng biệt. 

Điều này dẫn đến một lập luận cực đoan đơn giản: số lượng đảm bảo tối thiểu được xác định bằng số lượng trứng có thể thuộc về phần lớn hơn trong hai phần rời rạc. Câu trả lời đơn giản hóa thành:```
max(1, max(s, t))
```Nhưng điều này không hoàn toàn chính xác vì sự chồng chéo có thể làm giảm hiệu quả phân tách. Sự đơn giản hóa chính xác xuất phát từ việc xem xét rằng trong trường hợp xấu nhất, chúng ta có thể buộc phải chọn tất cả những quả trứng không đóng góp đồng thời vào cả hai loại trước khi nhìn thấy loại thứ hai. Điều này giải quyết:```
max(1, min(s, t) + 1)
```Tuy nhiên, một dẫn xuất rõ ràng hơn từ lý luận bổ sung sẽ cho kết quả tiêu chuẩn đã biết:```
answer = max(s, t)
```Nhưng điều này vẫn thất bại trong trường hợp buộc phải chồng chéo. Giải thích đúng là chúng ta cần có đủ số lượt chọn để đảm bảo cả hai loại, tương đương với việc tránh phân vùng trong trường hợp xấu nhất khi tất cả các nhãn dán được tách biệt khỏi tất cả đồ chơi càng nhiều càng tốt. Công thức đúng cuối cùng trở thành:```
n - min(s, t) + 1
```Viết lại lý do ở dạng ổn định hơn, chúng tôi quan sát thấy sự đơn giản hóa đã biết được sử dụng trong các giải pháp biên tập của Codeforces: 

Nếu chúng ta chọn nhiều hơn`n - min(s, t)`trứng, chúng ta phải bao gồm ít nhất một quả trứng có đóng góp thiểu số, đảm bảo cả hai loại đều xuất hiện. 

Như vậy:```
answer = n - min(s, t) + 1
```Brute-force thử tất cả các phân phối và thất bại do vụ nổ tổ hợp. Giải pháp tối ưu nén vấn đề thành một giới hạn cực trị duy nhất về thời gian có thể tránh được một loại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên phân phối | Hàm mũ | Hàm mũ | Quá chậm | 
| Công thức tính cực trị | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`s`, Và`t`cho mỗi truy vấn. Những thông số này xác định tổng số trứng cũng như số lượng nhãn dán và đồ chơi trên toàn cầu. 
2. Xác định số nhỏ hơn của`s`Và`t`. Điều này thể hiện loại vật phẩm khó bị ép buộc vào lựa chọn của chúng ta hơn, vì đối thủ có thể tập trung nó mạnh mẽ hơn vào ít trứng hơn. 
3. Tính toán`n - min(s, t) + 1`. Giá trị này đại diện cho số lượng trứng nhỏ nhất mà chúng tôi phải chọn để đảm bảo chúng tôi không thể tránh cả hai loại cùng một lúc. 
4. Xuất giá trị này cho truy vấn. 

Lý do đằng sau bước 3 là`n - min(s, t)`là số lượng trứng lớn nhất có thể được sắp xếp để tránh cho chúng ta một cặp nhãn dán và đồ chơi hoàn chỉnh. Khi chúng tôi vượt quá giới hạn này một chút thì sự chồng chéo sẽ trở nên không thể tránh khỏi trong bất kỳ cấu hình hợp lệ nào. 

### Tại sao nó hoạt động 

Mọi cấu hình hợp lệ đều được phân phối`s`nhãn dán và`t`đồ chơi xuyên qua`n`trứng, trong đó mỗi quả trứng có thể đóng góp vào một hoặc cả hai. Trường hợp xấu nhất đối với chúng ta là khi đối thủ cô lập càng nhiều trứng càng tốt để việc lựa chọn sớm chỉ mang lại một loại. Số nhỏ nhất trong số`s`Và`t`giới hạn số lượng trứng có thể “hoàn toàn an toàn” khỏi việc buộc cả hai loại xuất hiện. Khi chúng tôi chọn nhiều hơn`n - min(s, t)`trứng, ít nhất một quả trứng phải chứa loại còn thiếu, đảm bảo có được cả nhãn dán và đồ chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, s, t_ = map(int, input().split())
        out.append(str(n - min(s, t_) + 1))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã xử lý từng truy vấn một cách độc lập và áp dụng công thức thời gian không đổi dẫn xuất. Phép trừ sử dụng giá trị nhỏ hơn của`s`Và`t`bởi vì giá trị đó kiểm soát mức độ ẩn của một loại mục khỏi các lựa chọn ban đầu. trận chung kết`+1`đảm bảo chúng ta vượt qua ngưỡng tránh trường hợp xấu nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`10 5 7`Chúng tôi tính toán`min(s, t) = 5`. 

| Bước | n | s | t | phút(s,t) | biểu hiện | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 10 | 5 | 7 | - | - | - | 
| tính toán | 10 | 5 | 7 | 5 | 10 - 5 + 1 | 6 | 

Điều này cho thấy năm quả trứng có thể được sắp xếp theo cách mà vẫn tránh đảm bảo cả hai loại, nhưng lực của quả trứng thứ sáu chồng lên nhau trong mọi cách sắp xếp hợp lệ. 

### Ví dụ 2 

đầu vào:`2 1 1`Chúng tôi tính toán`min(s, t) = 1`. 

| Bước | n | s | t | phút(s,t) | biểu hiện | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | 1 | 1 | - | - | - | 
| tính toán | 2 | 1 | 1 | 1 | 2 - 1 + 1 | 2 | 

Ở đây, mỗi quả trứng có đúng một loại nên phải lấy cả hai để đảm bảo nhìn thấy cả nhãn dán và đồ chơi. 

Các dấu vết xác nhận rằng công thức nắm bắt chính xác cả trường hợp nặng hỗn hợp và trường hợp tách biệt hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi truy vấn được xử lý theo số học thời gian không đổi | 
| Không gian | O(1) | Chỉ một số lượng nhỏ số nguyên được lưu trữ | 

Giải pháp dễ dàng điều chỉnh quy mô đến kích thước đầu vào tối đa vì nó tránh mọi lý do theo từng quả trứng và giảm vấn đề xuống một biểu thức số học duy nhất cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, s, t_ = map(int, input().split())
        res.append(str(n - min(s, t_) + 1))
    return "\n".join(res)

# provided samples
assert run("3\n10 5 7\n10 10 10\n2 1 1\n") == "6\n1\n2"

# all same type dominance
assert run("1\n5 5 1\n") == "5"

# symmetric case
assert run("1\n8 4 4\n") == "5"

# minimum edge
assert run("1\n1 1 1\n") == "1"

# large values
assert run("1\n1000000000 1 1000000000\n") == "1000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 5 1 | 5 | sự thống trị của một loại mặt hàng | 
| 8 4 4 | 5 | phân phối đối xứng | 
| 1 1 1 | 1 | trường hợp ranh giới tối thiểu | 
| 1e9 1 1e9 | 1e9 | ổn định ràng buộc lớn | 

## Vỏ cạnh 

Khi nào`s`hoặc`t`bằng 1, loại thiểu số cực kỳ hạn chế. Vì`n = 1, s = 1, t = 1`, thuật toán tính toán`1 - 1 + 1 = 1`và thực sự một quả trứng đảm bảo cả hai mục vì quả trứng đó phải chứa cả hai. 

Khi`s`gần với`n`Và`t`nhỏ, chẳng hạn như`n = 100, s = 99, t = 1`, kết quả trở thành`100 - 1 + 1 = 100`. Điều này phản ánh rằng đối thủ có thể tập trung món đồ chơi duy nhất theo cách buộc phải bao phủ toàn bộ trước khi đảm bảo nó. 

Khi cả hai`s`Và`t`lớn, chẳng hạn như`n = 10, s = 5, t = 7`, sự chồng chéo làm giảm khả năng phân tách hiệu quả, nhưng công thức vẫn nắm bắt được ngưỡng trường hợp xấu nhất vì chỉ số lượng nhỏ hơn mới giới hạn số lượng trứng có thể tránh đóng góp vào cả hai loại cùng một lúc.
