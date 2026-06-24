---
title: "CF 105307C - Đũa"
description: "Chúng ta được cấp một tập hợp các “cặp” đũa giống hệt nhau, trong đó mỗi cặp được đặc trưng bởi một chiều dài nguyên duy nhất. Từ mỗi cặp chúng ta có thể coi hai chiếc đũa là hai cạnh bằng nhau của một hình chữ nhật."
date: "2026-06-23T14:46:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105307
codeforces_index: "C"
codeforces_contest_name: "ICPC 2024 Thailand - Chulalongkorn University Internal Round"
rating: 0
weight: 105307
solve_time_s: 65
verified: true
draft: false
---

[CF 105307C - Đũa](https://codeforces.com/problemset/problem/105307/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các “cặp” đũa giống hệt nhau, trong đó mỗi cặp được đặc trưng bởi một chiều dài nguyên duy nhất. Từ mỗi cặp chúng ta có thể coi hai chiếc đũa là hai cạnh bằng nhau của một hình chữ nhật. Để tạo thành một hình chữ nhật, chúng ta phải chọn hai cặp khác nhau: một cặp sẽ cung cấp các cạnh thẳng đứng và cặp còn lại sẽ cung cấp các cạnh ngang. Nếu chúng ta chọn một cặp có chiều dài`a`và một cặp khác có chiều dài`b`, diện tích hình chữ nhật trở thành`a × b`. 

Nhiệm vụ là tối đa hóa tích này trên tất cả các lựa chọn của hai cặp khác nhau. 

Kích thước đầu vào nhỏ, tối đa 100 cặp. Điều này ngay lập tức loại trừ mọi lo ngại về các ràng buộc phức tạp bậc cao. Ngay cả giải pháp O(N³) cũng có thể vượt qua một cách thoải mái, vì trường hợp xấu nhất chỉ xảy ra khoảng một triệu thao tác. Tuy nhiên, sự hiện diện của các giá trị lớn lên tới 10⁹ có nghĩa là chúng ta phải cẩn thận với phép nhân số nguyên, mặc dù Python xử lý các số nguyên lớn một cách an toàn. 

Một điểm tinh tế là chúng ta chọn từng đôi chứ không phải từng đôi đũa riêng lẻ. Điều này có nghĩa là mỗi độ dài đã chọn sẽ được lấy nguyên trạng từ danh sách các độ dài cặp. Không cần phải tách hoặc ghép từng que riêng lẻ. 

Các trường hợp Edge chủ yếu liên quan đến cấu trúc hơn là hiệu suất. Nếu tất cả các chiều dài bằng nhau thì hai cặp bất kỳ sẽ tạo ra cùng một hình chữ nhật. Nếu có chính xác hai cặp thì câu trả lời đơn giản là tích của chúng. Nếu các giá trị tối đa xuất hiện nhiều lần, chúng tôi phải đảm bảo chọn hai lần xuất hiện riêng biệt có cùng giá trị tối đa, vì không được phép sử dụng cùng một cặp hai lần ngay cả khi các giá trị khớp nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử từng cặp chỉ số riêng biệt. Đối với mỗi cặp`(i, j)`, tính toán`l[i] * l[j]`và theo dõi tối đa. Điều này đúng vì mọi hình chữ nhật hợp lệ đều tương ứng chính xác với lựa chọn gồm hai cặp riêng biệt và chúng tôi đánh giá tất cả các lựa chọn đó. 

Cách tiếp cận bạo lực này chạy trong thời gian O(N2). Với N ≤ 100, điều này có nghĩa là có nhiều nhất 4950 phép nhân, điều này không đáng kể. Vì vậy, nói đúng ra, điều này đã trôi qua. 

Tuy nhiên, chúng ta vẫn có thể đơn giản hóa lý luận bằng cách chú ý đến điều gì thực sự quan trọng. Vì chúng ta đang tối đa hóa tích của hai số được chọn từ danh sách nên giải pháp tối ưu phải bao gồm hai giá trị lớn nhất trong mảng. Tích của bất kỳ phần tử nhỏ hơn nào có mức tối đa không thể vượt quá tích của mức tối đa thứ hai với mức tối đa. Vì vậy, sắp xếp hoặc theo dõi hai giá trị trên cùng là đủ. 

Điều này làm giảm vấn đề quét một lần và giữ các giá trị lớn nhất và lớn thứ hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê cặp Brute Force | O(N2) | O(1) | Đã chấp nhận | 
| Theo dõi hai giá trị hàng đầu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào giải pháp tuyến tính trích xuất hai giá trị lớn nhất. 

1. Khởi tạo hai biến`first`Và`second`về không. Chúng sẽ lưu trữ độ dài lớn nhất và lớn thứ hai được thấy cho đến nay. Chúng tôi sử dụng số 0 vì tất cả các giá trị đầu vào đều dương. 
2. Lặp lại qua từng độ dài`x`trong danh sách đầu vào. 
3. Nếu`x`lớn hơn hoặc bằng`first`, sự thay đổi`first`vào trong`second`, sau đó đặt`first = x`. Điều này đảm bảo mức tối đa trước đó không bị mất và vẫn là ứng cử viên cho vị trí thứ hai. 
4. Ngược lại, nếu`x`lớn hơn`second`, cập nhật`second = x`. Điều này theo dõi giá trị tốt nhất không phải là giá trị tối đa. 
5. Sau khi xử lý tất cả các giá trị, tính kết quả như sau:`first * second`. 

Sự tinh tế quan trọng là xử lý các bản sao của giá trị tối đa. Nếu mức tối đa xuất hiện ít nhất hai lần, quy trình này sẽ đặt chính xác cả hai`first`Và`second`đến mức tối đa đó, tạo ra kết quả bình phương chính xác. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố nào của mảng,`first`Và`second`đại diện cho các giá trị lớn nhất và lớn thứ hai gặp phải cho đến nay. Bất kỳ phần tử nào trong tương lai đều trở thành phần tử tối đa mới hoặc cạnh tranh vị trí thứ hai. Bất biến này đảm bảo rằng sau khi xử lý tất cả các phần tử, không có cặp giá trị nào trong mảng có thể có tích lớn hơn`first × second`, vì bất kỳ cặp ứng cử viên nào cũng phải bao gồm các giá trị nhỏ hơn hoặc bằng hai giá trị cực đại này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    first = 0
    second = 0
    
    for x in arr:
        if x >= first:
            second = first
            first = x
        elif x > second:
            second = x
    
    print(first * second)

if __name__ == "__main__":
    solve()
```Giải pháp đọc danh sách, sau đó duy trì hai cực đại đang chạy. Chi tiết triển khai chính là`>=`tình trạng khi cập nhật`first`. Điều này đảm bảo rằng các giá trị tối đa trùng lặp được truyền chính xác vào`second`, điều này cần thiết cho những trường hợp như`[3, 3]`, trong đó cả hai giá trị phải được sử dụng. 

Chúng tôi tránh sắp xếp vì đó là chi phí không cần thiết, mặc dù điều đó cũng đúng. Cập nhật một lần đơn giản hơn và tránh tốn thêm bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`3 1 2 3`Chúng tôi theo dõi`first`Và`second`: 

| x | đầu tiên | thứ hai | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | cập nhật lần đầu | 
| 2 | 2 | 1 | cập nhật đầu tiên, cũ đầu tiên trở thành thứ hai | 
| 3 | 3 | 2 | cập nhật đầu tiên, cũ đầu tiên trở thành thứ hai | 

Câu trả lời cuối cùng là`3 × 2 = 6`. 

Điều này xác nhận rằng thuật toán luôn bảo toàn hai giá trị hàng đầu được thấy cho đến nay. 

### Mẫu 2 

đầu vào:`4 1 2 3 3`| x | đầu tiên | thứ hai | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | cập nhật lần đầu | 
| 2 | 2 | 1 | cập nhật lần đầu | 
| 3 | 3 | 2 | cập nhật lần đầu | 
| 3 | 3 | 3 | cập nhật tối đa trùng lặp thứ hai | 

Câu trả lời cuối cùng là`3 × 3 = 9`. 

Điều này chứng tỏ tầm quan trọng của việc xử lý các bản sao ở mức tối đa một cách chính xác. Nếu không có`>=`tình trạng,`second`sẽ không chính xác vẫn là 2, tạo ra 6 thay vì 9. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi giá trị được xử lý một lần trong một lần quét | 
| Không gian | O(1) | Chỉ có hai biến được duy trì bất kể kích thước đầu vào | 

Các ràng buộc cho phép tối đa 100 giá trị, vì vậy giải pháp này thấp hơn nhiều so với giới hạn. Ngay cả cách tiếp cận bậc hai cũng sẽ an toàn, nhưng quét tuyến tính sạch hơn và trực tiếp hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    first = 0
    second = 0
    
    for x in arr:
        if x >= first:
            second = first
            first = x
        elif x > second:
            second = x
    
    return str(first * second)

# provided samples
assert run("3\n1 2 3\n") == "6"
assert run("4\n1 2 3 3\n") == "9"

# custom cases
assert run("2\n5 7\n") == "35", "simple two elements"
assert run("3\n10 10 1\n") == "100", "duplicate max handling"
assert run("5\n1 1 1 1 1\n") == "1", "all equal values"
assert run("6\n9 8 7 6 5 4\n") == "72", "descending order"
assert run("5\n1 1000000000 2 3 4\n") == "4000000000", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 5 7`| 35 | trường hợp không tầm thường tối thiểu | 
|`3 / 10 10 1`| 100 | xử lý tối đa trùng lặp | 
|`5 / 1 1 1 1 1`| 1 | tất cả các giá trị bằng nhau | 
|`6 / 9 8 7 6 5 4`| 72 | ổn định trật tự giảm dần | 
|`5 / 1 1000000000 2 3 4`| 4000000000 | phép nhân giá trị lớn | 

## Vỏ cạnh 

Đối với trường hợp giá trị lớn nhất xuất hiện nhiều lần, hãy xem xét đầu vào`3 3 3`. Quá trình quét tiến hành bằng cách thiết lập`first = 3`,`second = 0`trên phần tử đầu tiên. Vào ngày thứ hai`3`, điều kiện`x >= first`kích hoạt một lần nữa, chuyển`first`vào trong`second`và đặt cả hai thành 3. Phần tử cuối cùng không thay đổi gì cả. Kết quả trở thành`3 × 3 = 9`, điều này đúng vì chúng ta được phép chọn hai cặp khác nhau ngay cả khi giá trị của chúng giống hệt nhau. 

Đối với một chuỗi tăng nghiêm ngặt như`1 2 3`, thuật toán tự nhiên kết thúc bằng`first = 3`Và`second = 2`. Sản phẩm`6`tương ứng với việc chọn hai độ dài cặp có sẵn lớn nhất, điều này là tối ưu vì bất kỳ cặp nào nhỏ hơn sẽ làm giảm một hệ số mà không làm tăng hệ số kia.
