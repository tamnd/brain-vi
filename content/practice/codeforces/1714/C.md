---
title: "CF 1714C - Số đa dạng tối thiểu"
description: "Chúng ta được cung cấp tổng mục tiêu cho các chữ số và với mỗi truy vấn, chúng ta cần xây dựng số nguyên dương nhỏ nhất có thể có các chữ số khác nhau và tổng các chữ số của nó bằng mục tiêu đó."
date: "2026-06-09T20:04:29+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1714
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 811 (Div. 3)"
rating: 800
weight: 1714
solve_time_s: 92
verified: true
draft: false
---

[CF 1714C - Số đa dạng tối thiểu](https://codeforces.com/problemset/problem/1714/C) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp tổng mục tiêu cho các chữ số và với mỗi truy vấn, chúng ta cần xây dựng số nguyên dương nhỏ nhất có thể có các chữ số khác nhau và tổng các chữ số của nó bằng mục tiêu đó. Trong số tất cả các số hợp lệ thỏa mãn hai ràng buộc này, chúng ta muốn số nhỏ nhất theo thứ tự số thông thường. 

Một số chỉ “hợp lệ” nếu mỗi chữ số xuất hiện nhiều nhất một lần, vì vậy về cơ bản chúng ta đang chọn một tập hợp con gồm các chữ số riêng biệt từ 1 đến 9 và sắp xếp chúng thành số nhỏ nhất có thể. Các số 0 không giúp giảm thiểu bất cứ điều gì ở đây vì việc thêm số 0 sẽ chỉ làm tăng số lượng chữ số mà không góp phần vào hiệu quả tính tổng và cấu trúc cũng tự nhiên ưu tiên các chữ số khác 0. 

Các ràng buộc cực kỳ nhỏ: tổng tối đa là 45 và có nhiều nhất 45 trường hợp thử nghiệm. Về mặt lý thuyết, việc tìm kiếm mạnh mẽ trên các tập hợp con các chữ số là khả thi vì chỉ có$2^9 = 512$các tập hợp con, nhưng chúng tôi cũng phải đảm bảo việc đặt hàng tạo ra số lượng nhỏ nhất, điều này bổ sung thêm việc kiểm tra tổ hợp cho mỗi tập hợp con. 

Một cách tiếp cận đơn giản có thể thử hoán vị hoặc quay lui các chữ số để khớp với tổng. Mặc dù đúng, nhưng nó có nguy cơ khám phá những hoán vị không cần thiết chỉ khác nhau về thứ tự, mặc dù thứ tự có sự lựa chọn tối ưu xác định. Một sai lầm tiềm ẩn khác là tham lam lấy các chữ số nhỏ nhất trước mà không xem xét rằng các chữ số lớn hơn có thể cần thiết để đạt được tổng chính xác với các ràng buộc về tính duy nhất. 

Trường hợp khó phát hiện nhất là khi tổng lớn, chẳng hạn như 45. Cách tiếp cận tham lam cố gắng đóng gói các chữ số nhỏ trước tiên không thành công vì nó không thể đạt được tổng yêu cầu nếu không trùng lặp, chẳng hạn như sử dụng lặp đi lặp lại 9. Cấu trúc đúng phải cân bằng giữa tính duy nhất với tính khả thi của tổng. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là xem xét mọi tập hợp con các chữ số từ 1 đến 9, kiểm tra xem tổng của nó có bằng mục tiêu hay không và trong số tất cả các tập hợp con hợp lệ sẽ xây dựng số nhỏ nhất bằng cách sắp xếp các chữ số và diễn giải chúng dưới dạng số. Điều này hiệu quả vì không gian tìm kiếm rất nhỏ nhưng vẫn liên quan đến việc lặp lại trên 512 tập hợp con và tính tổng nhiều lần, điều này là không cần thiết đối với cấu trúc. 

Quan sát quan trọng là vấn đề tương đương với việc chọn các chữ số riêng biệt có tổng bằng chính xác$s$, rồi sắp xếp chúng theo thứ tự tăng dần để sao cho số kết quả là nhỏ nhất. Sau khi tập hợp các chữ số được cố định, cách sắp xếp tốt nhất luôn được sắp xếp tăng dần, vì các chữ số nhỏ hơn sẽ xuất hiện sớm hơn để giảm thiểu đồng thời giá trị từ điển và số. 

Điều này làm giảm vấn đề lựa chọn tập hợp con có cấu trúc đơn điệu mạnh. Thay vì khám phá tất cả các tập hợp con, chúng ta có thể tham lam xây dựng câu trả lời từ chữ số lớn nhất trở xuống. Chúng tôi thử các chữ số từ 9 đến 1 và bao gồm một chữ số nếu nó không vượt quá tổng còn lại. Điều này có tác dụng vì việc chọn chữ số lớn hơn trước đó sẽ duy trì tính linh hoạt cho các chữ số nhỏ hơn còn lại và đảm bảo chúng tôi giảm thiểu số lượng chữ số trước tiên với những đóng góp có giá trị cao. 

Quá trình tham lam là tối ưu vì chúng ta luôn thích ít chữ số hơn là nhiều chữ số hơn và trong số các số bằng nhau, việc đặt các chữ số nhỏ hơn trước đó sẽ cho số nhỏ hơn. Việc lựa chọn giảm dần đảm bảo chúng ta không bao giờ bỏ sót một biểu diễn khả thi nào, vì bất kỳ tổng nào lên tới 45 đều có thể được phân tách duy nhất thành các chữ số riêng biệt được chọn theo cách này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(512 · 9) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tham lam từ 9 đến 1 | O(9) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ chữ số 9 và giảm xuống số 1, giữ nguyên số tiền còn lại$s$. Chúng tôi làm điều này vì các chữ số lớn hơn sẽ giảm số lượng chữ số cần thiết, điều này có lợi cho việc giảm thiểu số cuối cùng. 
2. Với mỗi chữ số$d$, kiểm tra xem$d \leq s$. Nếu có, hãy chọn chữ số này và trừ nó khỏi$s$. Điều này đảm bảo rằng mọi chữ số được chọn đều đóng góp chính xác và không vi phạm tổng yêu cầu còn lại. 
3. Tiếp tục quá trình này cho đến khi tất cả các chữ số đã được xem xét. Cuối cùng, số tiền còn lại trở thành 0 hoặc không có sự kết hợp hợp lệ nào tồn tại. Với những hạn chế$s \le 45$, một phân tách hợp lệ luôn tồn tại. 
4. Thu thập tất cả các chữ số đã chọn. Các chữ số này tạo thành một tập hợp con hợp lệ với các giá trị riêng biệt và tổng chính xác. 
5. Sắp xếp các chữ số đã chọn theo thứ tự tăng dần. Bước này là cần thiết vì mặc dù chúng tôi đã chọn các chữ số một cách tham lam theo thứ tự giảm dần để đảm bảo tính khả thi, nhưng giá trị số tối thiểu yêu cầu sắp xếp tăng dần. 
6. Xuất ra kết quả nối các chữ số đã được sắp xếp. 

### Tại sao nó hoạt động 

Thuật toán xây dựng một tập hợp con các chữ số có tổng chính xác bằng$s$, luôn ưu tiên các chữ số lớn hơn khi có thể. Điều này đảm bảo rằng chúng tôi giảm thiểu số lượng chữ số trong kết quả vì các chữ số lớn hơn bao gồm nhiều tổng hơn trên mỗi phần tử. Sau khi tập hợp con được cố định, việc sắp xếp nó theo thứ tự tăng dần sẽ mang lại số nhỏ nhất có thể cho tập hợp chữ số đó. Bất kỳ tập hợp con thay thế nào cũng sử dụng nhiều chữ số hơn hoặc thay thế chữ số lớn hơn bằng chữ số nhỏ hơn, điều này buộc phải bao gồm các chữ số bổ sung và dẫn đến tổng số lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = int(input())
    
    digits = []
    
    for d in range(9, 0, -1):
        if d <= s:
            digits.append(d)
            s -= d
    
    digits.sort()
    print("".join(map(str, digits)))
```Việc thực hiện trực tiếp theo sau việc xây dựng tham lam. Chúng ta lặp lại từ 9 xuống 1, trừ các chữ số bất cứ khi nào có thể. Điều này đảm bảo chúng tôi gộp số tiền một cách hiệu quả với các chữ số có sẵn lớn nhất trước tiên. Sau khi xây dựng tập hợp, việc sắp xếp là cần thiết vì thứ tự lựa chọn tham lam không phải là thứ tự số cuối cùng cần thiết để đạt được mức tối thiểu. 

Một lỗi phổ biến là bỏ qua bước sắp xếp và in các chữ số theo thứ tự chọn ngược. Điều đó tạo ra số tiền hợp lệ nhưng không phải là số tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1: s = 20 

Chúng tôi theo dõi các chữ số đã chọn trong khi lặp từ 9 đến 1. 

| Chữ số | Số tiền còn lại (trước) | Lấy chữ số? | Số tiền còn lại (sau) | Bộ được chọn | 
| --- | --- | --- | --- | --- | 
| 9 | 20 | vâng | 11 | {9} | 
| 8 | 11 | vâng | 3 | {9,8} | 
| 7 | 3 | không | 3 | {9,8} | 
| 6 | 3 | không | 3 | {9,8} | 
| 5 | 3 | không | 3 | {9,8} | 
| 4 | 3 | không | 3 | {9,8} | 
| 3 | 3 | vâng | 0 | {9,8,3} | 

Sau khi sắp xếp: 3, 8, 9 → kết quả là 389. 

Dấu vết này cho thấy phương pháp tham lam ưu tiên các chữ số lớn như thế nào trong khi vẫn đảm bảo có thể đạt được tổng chính xác. 

### Ví dụ 2: s = 10 

| Chữ số | Số tiền còn lại (trước) | Lấy chữ số? | Số tiền còn lại (sau) | Bộ được chọn | 
| --- | --- | --- | --- | --- | 
| 9 | 10 | vâng | 1 | {9} | 
| 8 | 1 | không | 1 | {9} | 
| 7 | 1 | không | 1 | {9} | 
| 6 | 1 | không | 1 | {9} | 
| 5 | 1 | không | 1 | {9} | 
| 4 | 1 | không | 1 | {9} | 
| 3 | 1 | không | 1 | {9} | 
| 2 | 1 | không | 1 | {9} | 
| 1 | 1 | vâng | 0 | {9,1} | 

Sau khi sắp xếp: 1, 9 → đầu ra là 19. 

Ví dụ này nhấn mạnh rằng lựa chọn tham lam có thể để lại một phần dư nhỏ và thuật toán sẽ điền chính xác vào đó bằng chữ số nhỏ nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(9 · t) | Đối với mỗi trường hợp thử nghiệm, chúng tôi quét các chữ số từ 9 đến 1 một lần | 
| Không gian | O(1) | Chỉ một danh sách nhỏ có tối đa 9 chữ số được lưu trữ | 

Việc tính toán diễn ra liên tục trong thời gian cho mỗi trường hợp thử nghiệm và dễ dàng nằm trong giới hạn, ngay cả đối với các truy vấn lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s = int(input())
        digits = []
        for d in range(9, 0, -1):
            if d <= s:
                digits.append(d)
                s -= d
        digits.sort()
        out.append("".join(map(str, digits)))
    return "\n".join(out)

# provided samples
assert run("4\n20\n8\n45\n10\n") == "389\n8\n123456789\n19"

# custom cases
assert run("1\n1\n") == "1"
assert run("1\n9\n") == "9"
assert run("1\n17\n") == "89"
assert run("1\n2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp khác không tối thiểu | 
| 9 | 9 | ranh giới trên một chữ số | 
| 17 | 89 | cần sự kết hợp tham lam | 
| 2 | 2 | tổng không tầm thường nhỏ nhất | 

## Vỏ cạnh 

Đối với đầu vào nhỏ nhất$s = 1$, thuật toán lặp từ 9 trở xuống, loại bỏ tất cả các chữ số ngoại trừ 1 và chỉ chọn chữ số 1. Việc sắp xếp không quan trọng và đầu ra là 1. 

Đối với trường hợp gần tối đa như$s = 45$, mọi chữ số từ 9 đến 1 đều được chọn vì tổng của chúng khớp chính xác với 45. Tập kết quả là {9,8,7,6,5,4,3,2,1} và việc sắp xếp tạo ra 123456789. Thuật toán xử lý việc này một cách rõ ràng vì không có chữ số nào bị bỏ qua. 

Đối với các trường hợp tổng tạo ra sự kết hợp không rõ ràng như$s = 17$, quá trình tham lam chọn 9 trước, để lại 8, sau đó chọn chính xác 8, tạo ra {9,8}. Việc sắp xếp mang lại 89, là kết quả tối ưu vì bất kỳ phương án thay thế nào cũng sẽ yêu cầu nhiều chữ số hơn và do đó số lượng lớn hơn.
