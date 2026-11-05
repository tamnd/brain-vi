---
title: "CF 1186A - Vus the Cossack và một cuộc thi"
description: "Chúng tôi được đưa ra một vấn đề phân phối nhỏ. Có một số người tham gia một cuộc thi và mỗi người tham gia phải nhận được hai vật phẩm: một cây bút và một cuốn sổ."
date: "2026-06-13T12:19:48+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1186
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 571 (Div. 2)"
rating: 800
weight: 1186
solve_time_s: 188
verified: true
draft: false
---

[CF 1186A - Vus the Cossack và một cuộc thi](https://codeforces.com/problemset/problem/1186/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một vấn đề phân phối nhỏ. Có một số người tham gia một cuộc thi và mỗi người tham gia phải nhận được hai vật phẩm: một cây bút và một cuốn sổ. Người tổ chức có một kho bút và vở cố định, và nhiệm vụ là quyết định xem liệu có thể làm hài lòng tất cả mọi người cùng một lúc hay không. 

Chính thức, mỗi người trong số n người tham gia cần một cây bút và một cuốn sổ, vì vậy tổng nhu cầu là n cây bút và n cuốn vở. Câu hỏi đơn giản là liệu số lượng m và k sẵn có có đủ để đáp ứng đồng thời hai yêu cầu độc lập này hay không. 

Các ràng buộc cực kỳ nhỏ, với tất cả các giá trị được giới hạn bởi 100. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa hoặc cấu trúc dữ liệu phức tạp. Bất kỳ giải pháp nào thực hiện một số lượng thao tác không đổi sẽ là đủ, vì ngay cả một mô phỏng đơn giản cũng chỉ thực hiện được vài trăm bước. 

Các trường hợp thất bại có ý nghĩa duy nhất phát sinh khi một trong hai nguồn lực không đủ ngay cả khi nguồn lực kia dồi dào. Ví dụ: nếu n = 8, m = 5, k = 100, chỉ riêng bút thôi là không thể thực hiện được nhiệm vụ mặc dù sổ tay là quá đủ. Tương tự, nếu m = 100, k = 2, n = 5 thì chỉ riêng sổ ghi chép đã cản trở tính khả thi. Một sai lầm tinh tế hơn một chút sẽ xảy ra nếu một người cố gắng so sánh không chính xác tổng số mục m + k với 2n, điều này sẽ cho phép phân phối không hợp lệ khi thiếu hoàn toàn một danh mục. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng chỉ định các mục cho từng người tham gia. Đối với mỗi người trong số n người tham gia, chúng tôi sẽ giảm đi một cây bút và một cuốn sổ từ kho và kiểm tra xem liệu chúng tôi có bao giờ xuống dưới 0 hay không. Điều này đúng vì nó mô phỏng trực tiếp quá trình phân phối. 

Tuy nhiên, mặc dù sức mạnh vũ phu này rất đơn giản nhưng nó không cần thiết. Nó thực hiện n lần lặp, mỗi lần thực hiện công việc liên tục, điều này vẫn ổn ở đây nhưng dư thừa về mặt khái niệm. Quan trọng hơn, nó che khuất cấu trúc của vấn đề: mỗi người tham gia tiêu thụ độc lập chính xác một đơn vị từ mỗi nhóm tài nguyên, nghĩa là điều kiện khả thi sẽ phân tách thành hai lần kiểm tra độc lập. 

Cái nhìn sâu sắc quan trọng là không có sự tương tác giữa những người tham gia ngoài việc đếm. Khi chúng tôi nhận ra rằng mỗi người tham gia sử dụng chính xác một cây bút và một cuốn sổ, toàn bộ vấn đề sẽ giảm xuống việc xác minh hai bất đẳng thức: liệu bút có ít nhất là n hay không và liệu vở có ít nhất là n hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các số nguyên n, m và k tương ứng là số người tham gia, số bút và số vở. 
2. Kiểm tra xem số lượng bút có ít nhất là n hay không. Điều kiện này đảm bảo rằng chúng ta có thể chỉ định một cây bút cho mỗi người tham gia mà không bị hết bút. 
3. Kiểm tra xem số lượng vở ghi ít nhất có phải là n không. Điều này đảm bảo điều kiện khả thi tương tự cho máy tính xách tay. 
4. Nếu cả hai điều kiện được giữ đồng thời, xuất ra "Có". Nếu không thì xuất ra "No". 

### Tại sao nó hoạt động 

Mỗi người tham gia yêu cầu chính xác một đơn vị từ mỗi nguồn trong số hai nguồn độc lập. Không có sự chia sẻ, tái sử dụng hoặc thay thế giữa bút và vở. Điều này có nghĩa là tính khả thi được xác định hoàn toàn bằng việc mỗi nhóm tài nguyên có đáp ứng được nhu cầu n hay không. Vì các nhiệm vụ là độc lập và giống hệt nhau đối với mọi người tham gia, nên việc đáp ứng cả hai ràng buộc chung sẽ đảm bảo phân phối hợp lệ và việc không thực hiện được một trong hai ràng buộc sẽ khiến điều đó không thể thực hiện được bất kể tài nguyên kia được phân phối như thế nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    if m >= n and k >= n:
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    solve()
```Mã đọc ba giá trị đầu vào và kiểm tra trực tiếp hai điều kiện cần thiết. Việc so sánh được thực hiện độc lập đối với bút và sổ ghi chép vì không nguồn nào có thể bù đắp được sự thiếu hụt ở nguồn kia. Đầu ra được in ngay lập tức dựa trên điều kiện boolean này, làm cho lời giải có thời gian không đổi. 

Một lỗi triển khai phổ biến là chỉ kiểm tra m + k >= 2 * n, bỏ qua yêu cầu mỗi người tham gia phải nhận cả hai mục cùng một lúc. Một sai lầm khác là trừ đi một cách tham lam khỏi nhóm kết hợp, điều này cho phép trộn các tài nguyên một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 8 6
```| Bước | n | m | k | m ≥ n | k ≥ n | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 5 | 8 | 6 | - | - | - | 
| Kiểm tra bút | 5 | 8 | 6 | Đúng | - | - | 
| Kiểm tra sổ ghi chép | 5 | 8 | 6 | Đúng | Đúng | Có | 

Cả hai tài nguyên đều vượt quá hoặc phù hợp với số lượng người tham gia được yêu cầu, vì vậy mọi người tham gia đều có thể nhận được cả hai vật phẩm. 

### Ví dụ 2 

đầu vào:```
3 9 3
```| Bước | n | m | k | m ≥ n | k ≥ n | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 9 | 3 | - | - | - | 
| Kiểm tra bút | 3 | 9 | 3 | Đúng | - | - | 
| Kiểm tra sổ ghi chép | 3 | 9 | 3 | Đúng | Đúng | Có | 

Dù có rất nhiều bút nhưng sổ tay vẫn đáp ứng đúng yêu cầu, thế là đủ. 

Điều này khẳng định rằng sự bình đẳng là có thể chấp nhận được và chỉ có sự thiếu sót nghiêm ngặt mới gây ra thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có hai so sánh được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Bản chất thời gian không đổi dễ dàng phù hợp với các ràng buộc, vì kích thước đầu vào là không đáng kể và không cần phải lặp lại đối với những người tham gia. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, k = map(int, sys.stdin.readline().split())
    return "Yes" if m >= n and k >= n else "No"

# provided samples
assert run("5 8 6") == "Yes"
assert run("3 9 3") == "Yes"

# custom cases
assert run("1 1 1") == "Yes"
assert run("1 0 1") == "No"
assert run("10 100 9") == "No"
assert run("100 100 100") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | Có | Trường hợp hợp lệ tối thiểu | 
| 1 0 1 | Không | Thất bại do bút | 
| 10 100 9 | Không | Thất bại do máy tính xách tay | 
| 100 100 100 | Có | Trường hợp cân bằng tối đa | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một tài nguyên chính xác bằng n trong khi tài nguyên kia lớn hơn nhiều. Ví dụ: n = 4, m = 4, k = 100. Thuật toán kiểm tra m ≥ n và k ≥ n nên trả về True. Điều này phù hợp với thực tế vì mỗi người tham gia vẫn cần một cuốn sổ dù có thừa bút hay không. 

Một trường hợp khác là khi cả hai tài nguyên đều có tổng số lớn nhưng không cân bằng trên mỗi danh mục, chẳng hạn như n = 10, m = 19, k = 1. Mặc dù m + k là 20 vượt quá 2n, thuật toán trả về chính xác Không vì k < n. Từng bước, việc kiểm tra sổ ghi chép không thành công ngay lập tức, ngăn chặn việc phân phối không hợp lệ. 

Trường hợp cạnh cuối cùng là đầu vào nhỏ nhất n = 1, m = 0, k = 1. Điều kiện bút không thành công nên thuật toán đưa ra Không, phù hợp với thực tế là ngay cả một người tham gia cũng không thể được trang bị đầy đủ.
