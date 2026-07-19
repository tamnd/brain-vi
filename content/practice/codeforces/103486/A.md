---
title: "CF 103486A - Trình kiểm tra số ngẫu nhiên"
description: "Chúng ta được cho một dãy số nguyên và cần đánh giá xem nó có “cân bằng” về mặt tính chẵn lẻ hay không. Mỗi số là số lẻ hoặc số chẵn và chúng tôi chỉ cần đếm xem có bao nhiêu số thuộc mỗi loại."
date: "2026-07-03T06:20:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103486
codeforces_index: "A"
codeforces_contest_name: "The 15th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 103486
solve_time_s: 45
verified: true
draft: false
---

[CF 103486A - Trình kiểm tra số ngẫu nhiên](https://codeforces.com/problemset/problem/103486/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và cần đánh giá xem nó có “cân bằng” về mặt tính chẵn lẻ hay không. Mỗi số là số lẻ hoặc số chẵn và chúng tôi chỉ cần đếm xem có bao nhiêu số thuộc mỗi loại. 

Quy tắc rất nghiêm ngặt: tính số giá trị lẻ và số giá trị chẵn, sau đó lấy hiệu tuyệt đối của chúng. Nếu sự khác biệt đó nhiều nhất là một thì trình tạo được coi là chấp nhận được, nếu không thì không. 

Kích thước đầu vào lên tới 100.000 số, do đó, bất kỳ giải pháp nào quét mảng một lần đều đủ nhanh. Cách tiếp cận bậc hai sẽ cố gắng so sánh các phần tử theo cặp hoặc lọc liên tục danh sách, điều này sẽ thực hiện theo thứ tự các phép toán 10¹⁰ trong trường hợp xấu nhất và rõ ràng vượt quá giới hạn. Điều này ngay lập tức gợi ý rằng chúng ta chỉ cần một lượt duy nhất với công việc liên tục cho mỗi phần tử. 

Các trường hợp cạnh ở đây chủ yếu là về đầu vào nhỏ. Khi N bằng 1, câu trả lời luôn là “Tốt” vì số đếm là (1,0) hoặc (0,1) và hiệu chính xác là 1. Khi N bằng 2, các cấu hình như một lẻ và một chẵn là tốt, trong khi hai cấu hình có cùng số chẵn lẻ cũng tốt vì chênh lệch là 2 trừ 0 bằng 2, điều này không được phép. Vì vậy, “1 1” hoặc “2 2” sẽ xuất ra “Không tốt”, trong khi “1 2” sẽ xuất ra “Tốt”. Việc triển khai bất cẩn có thể chỉ kiểm tra nhầm xem cả hai số chẵn lẻ có tồn tại hay không, thay vì so sánh số lượng một cách chính xác. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là tách mảng thành hai nhóm: số lẻ và số chẵn, sau đó đếm rõ ràng cả hai nhóm bằng cách quét hoặc lọc nhiều lần. Người ta có thể tưởng tượng việc lặp lại tất cả các số và kiểm tra từng phần tử xem nó là số lẻ hay số chẵn và các bộ đếm tăng dần. Điều này đã đủ, nhưng thay vào đó, nếu ai đó liên tục lọc mảng hoặc tính toán lại số lượng bên trong các vòng lặp lồng nhau, thì độ phức tạp sẽ giảm xuống O(N²), vì mỗi lần truyền qua mảng là O(N) và nó có thể được lặp lại cho mỗi phần tử. 

Quan sát quan trọng là không có gì phụ thuộc vào thứ tự hoặc mối quan hệ giữa các phần tử. Mỗi số đóng góp độc lập vào đúng một trong hai bộ đếm. Khi chúng tôi nhận ra điều này, vấn đề sẽ chuyển thành một lần quét tuyến tính duy nhất trong đó chúng tôi duy trì hai số nguyên: một cho số lẻ và một cho số chẵn. Sau khi xử lý tất cả các số, chúng ta chỉ cần so sánh sự khác biệt của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai bộ đếm, một bộ đếm số lẻ và một bộ đếm số chẵn, cả hai đều được đặt bằng 0. Điều này chuẩn bị cho chúng ta tích lũy tần số trong một lần duy nhất. 
2. Lặp qua từng số trong mảng. Đối với mỗi giá trị, hãy xác định tính chẵn lẻ của nó bằng điều kiện`value % 2 == 0`. Hoạt động này là thời gian không đổi và không phụ thuộc vào kích thước đầu vào. 
3. Nếu số là số chẵn, hãy tăng bộ đếm số chẵn; nếu không thì tăng bộ đếm lẻ. Điều này đảm bảo mọi phần tử đều đóng góp chính xác một lần vào đúng danh mục. 
4. Sau khi xử lý tất cả các số, hãy tính chênh lệch tuyệt đối giữa hai bộ đếm. 
5. Nếu chênh lệch này nhỏ hơn hoặc bằng 1, xuất ra “Tốt”. Nếu không thì xuất ra “Không Tốt”. 

### Tại sao nó hoạt động 

Mỗi phần tử trong đầu vào thuộc về chính xác một trong hai tập hợp rời nhau: lẻ hoặc chẵn. Thuật toán duy trì số lượng chính xác cho cả hai bộ mà không có sự gần đúng hoặc chồng chéo. Vì mỗi phần tử được xử lý một lần và chỉ một lần nên bộ đếm cuối cùng là số chính xác của hai bộ. Quy tắc quyết định chỉ phụ thuộc vào các số lượng này, do đó tính chính xác xuất phát trực tiếp từ thực tế là số lượng là chính xác và không có chuyển đổi dữ liệu nào được thực hiện ngoài phân loại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    odd = 0
    even = 0
    
    for x in arr:
        if x % 2 == 0:
            even += 1
        else:
            odd += 1
    
    if abs(odd - even) <= 1:
        print("Good")
    else:
        print("Not Good")

if __name__ == "__main__":
    solve()
```Việc thực hiện theo thuật toán trực tiếp. Chúng tôi đọc tất cả các giá trị một lần, sau đó duy trì hai bộ đếm. Kiểm tra tính chẵn lẻ sử dụng modulo 2, an toàn cho tất cả các ràng buộc vì các giá trị lên tới 10⁹. Phép so sánh cuối cùng sử dụng sai phân tuyệt đối, đảm bảo tính đối xứng giữa số lẻ và số chẵn. 

Một điểm tinh tế là phân tích cú pháp đầu vào: chúng tôi đọc toàn bộ dòng thứ hai thành một danh sách trong một lệnh gọi. Điều này là an toàn vì N tối đa là 10⁵ nên mức sử dụng bộ nhớ là không đáng kể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```| Bước | Số | lẻ | Thậm chí | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 2 | 1 | 1 | 
| 3 | 3 | 2 | 1 | 
| 4 | 4 | 2 | 2 | 
| 5 | 5 | 3 | 2 | 

Lẻ cuối cùng = 3, chẵn = 2, chênh lệch = 1, do đó kết quả đầu ra là “Tốt”. 

Điều này cho thấy hành vi cân bằng dự định trong đó cả hai loại đều tăng trưởng gần như bằng nhau. 

### Ví dụ 2 

đầu vào:```
5
1 1 3 4 5
```| Bước | Số | lẻ | Thậm chí | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 1 | 2 | 0 | 
| 3 | 3 | 3 | 0 | 
| 4 | 4 | 3 | 1 | 
| 5 | 5 | 4 | 1 | 

Lẻ cuối cùng = 4, chẵn = 1, chênh lệch = 3, vì vậy đầu ra là “Không tốt”. 

Điều này nhấn mạnh việc phân phối sai lệch vi phạm ngưỡng như thế nào mặc dù cả hai số chẵn lẻ đều tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Chúng tôi quét mảng một lần và thực hiện công việc liên tục trên mỗi phần tử | 
| Không gian | O(1) | Chỉ có hai bộ đếm được lưu trữ bất kể kích thước đầu vào | 

Quá trình quét tuyến tính vừa vặn thoải mái trong các giới hạn cho N lên tới 100.000 và mức sử dụng bộ nhớ không đổi ngoại trừ mảng đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n1 2 3 4 5\n") == "Good"
assert run("5\n1 1 3 4 5\n") == "Not Good"

# minimum size
assert run("1\n2\n") == "Good"
assert run("1\n1\n") == "Good"

# perfectly balanced
assert run("4\n1 2 3 4\n") == "Good"

# all same parity (even)
assert run("4\n2 4 6 8\n") == "Not Good"

# all same parity (odd)
assert run("5\n1 3 5 7 9\n") == "Not Good"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 chẵn/lẻ | Tốt | ranh giới một phần tử | 
| trộn nhỏ | Tốt | tình trạng cân bằng | 
| tất cả thậm chí | Không Tốt | mất cân bằng cực độ | 
| tất cả đều kỳ quặc | Không Tốt | mất cân bằng cực độ | 

## Vỏ cạnh 

### Đầu vào một phần tử 

đầu vào:```
1
7
```Thuật toán khởi tạo lẻ = 0, chẵn = 0, sau đó đọc 7 là số lẻ, do đó số lẻ trở thành 1. Hiệu là 1 nên đầu ra là “Tốt”. Điều này xác nhận điều kiện biên trong đó kích thước đầu vào tối thiểu vẫn thỏa mãn quy tắc. 

### Tất cả các phần tử đều có tính chẵn lẻ giống hệt nhau 

đầu vào:```
6
2 4 6 8 10 12
```Bộ xử lý chẵn thành 6 và lẻ vẫn bằng 0. Chênh lệch tuyệt đối là 6, vượt quá 1, do đó đầu ra là “Không tốt”. Thuật toán tổng hợp chính xác số lượng mà không cần bất kỳ lý luận cặp nào. 

###Trường hợp gần cân bằng 

đầu vào:```
3
1 2 3
```Số lẻ trở thành 2 và thậm chí trở thành 1. Hiệu là 1, do đó đầu ra là “Tốt”. Điều này xác nhận rằng điều kiện ngưỡng được bao gồm và được xử lý chính xác bởi`<= 1`.
