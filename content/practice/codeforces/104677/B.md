---
title: "CF 104677B - Chiến tranh trên hai mặt trận"
description: "Chúng ta được cho hai nhóm riêng biệt gồm năm số nguyên. Mỗi nhóm đại diện cho năm người ở một bên của lớp học và mỗi người đóng góp một số điểm cố định. Darcy được phép chọn chính xác một trong hai nhóm."
date: "2026-06-29T14:32:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104677
codeforces_index: "B"
codeforces_contest_name: "Sugar Sweet \u2764\ufe0f"
rating: 0
weight: 104677
solve_time_s: 55
verified: true
draft: false
---

[CF 104677B - Chiến tranh trên hai mặt trận](https://codeforces.com/problemset/problem/104677/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai nhóm riêng biệt gồm năm số nguyên. Mỗi nhóm đại diện cho năm người ở một bên của lớp học và mỗi người đóng góp một số điểm cố định. 

Darcy được phép chọn chính xác một trong hai nhóm. Sau khi chọn một nhóm, anh ta loại bỏ chính xác một phần tử khỏi nhóm đó và thu thập tổng của bốn số còn lại. Nhiệm vụ là tối đa hóa số điểm anh ta có thể đạt được qua cả hai lựa chọn. 

Vì vậy, về mặt khái niệm, chúng tôi tính toán hai điểm ứng viên: đối với mỗi nhóm, chúng tôi lấy tổng tổng và trừ đi một phần tử đã chọn. Vì chúng ta được phép loại bỏ bất kỳ phần tử đơn lẻ nào, nên lựa chọn tối ưu trong một nhóm luôn là loại bỏ giá trị nhỏ nhất, vì điều đó bảo toàn tổng lớn nhất có thể còn lại. 

Kích thước đầu vào là cố định và nhỏ: tổng cộng chính xác là mười số. Điều này loại bỏ mọi lo ngại về hiệu quả và chuyển hoàn toàn trọng tâm sang việc xác định chính xác lựa chọn loại bỏ tốt nhất. 

Không có hạn chế mở rộng quy mô phức tạp. Bất kỳ giải pháp nào có thời gian không đổi cho mỗi đầu vào là đủ. Ngay cả việc liệt kê tất cả các lần xóa cũng sẽ diễn ra ngay lập tức. 

Chế độ thất bại tinh tế chính xuất phát từ việc quên rằng lựa chọn loại bỏ là độc lập trong mỗi nhóm. Một cách tiếp cận sai có thể cố gắng so sánh các phần tử giữa các nhóm hoặc giả định phần tử tốt nhất toàn cầu cần loại bỏ, điều này không phản ánh quy tắc chỉ chọn một nhóm. 

Một cách tiếp cận không chính xác cụ thể sẽ là luôn loại bỏ số lượng nhỏ nhất trên cả hai nhóm cộng lại. Ví dụ, nếu một nhóm được`[100, 100, 100, 100, 1]`và cái còn lại là`[50, 50, 50, 50, 50]`, loại bỏ mức tối thiểu toàn cầu (1) buộc phải chọn nhóm đầu tiên, cho 400, nhưng nhóm thứ hai mang lại 250, tệ hơn, vì vậy ví dụ này không phá vỡ nó. Tuy nhiên, nếu cấu trúc khác nhau, lý do như vậy có thể thất bại vì quyết định là “chọn một nhóm trước, sau đó loại bỏ trong đó”, chứ không phải “loại bỏ toàn bộ”. 

Cấu trúc đúng là tối ưu hóa cho mỗi nhóm theo sau là mức tối đa cuối cùng. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force rất đơn giản: đối với mỗi nhóm trong số hai nhóm, hãy thử loại bỏ từng phần tử trong số năm phần tử và tính tổng kết quả của bốn phần tử còn lại. Điều này tạo ra tổng cộng mười giá trị ứng cử viên và chúng tôi lấy giá trị tối đa. Điều này đúng vì nó liệt kê rõ ràng mọi hành động hợp lệ mà Darcy có thể thực hiện. 

Cách tiếp cận này chạy trong thời gian không đổi vì kích thước đầu vào là cố định. Ngay cả khi chúng tôi khái quát hóa nó thành n phần tử cho mỗi nhóm, nó sẽ yêu cầu O(n) cho mỗi nhóm, điều này vẫn không đáng kể đối với các ràng buộc n cho đến mức vừa phải. Sự dư thừa xuất phát từ việc tính toán lại các khoản tiền nhiều lần. 

Quan sát quan trọng là việc tính lại tổng số tiền đầy đủ là không cần thiết. Khi chúng ta biết tổng của một nhóm, việc xóa một phần tử chỉ đơn giản là trừ đi giá trị đó. Do đó, kết quả tốt nhất là tổng trừ đi phần tử nhỏ nhất trong nhóm đó. Điều này làm giảm mỗi nhóm thành một phép tính duy nhất: tổng và tối thiểu. 

Chúng tôi tính toán điểm của cả hai nhóm một cách độc lập và sau đó lấy điểm tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc năm số nguyên của nhóm đầu tiên và tính tổng và giá trị nhỏ nhất của chúng. Mức tối thiểu xác định yếu tố tốt nhất cần loại bỏ vì việc loại bỏ bất kỳ thứ gì lớn hơn sẽ lãng phí điểm số tiềm năng. 
2. Tính số điểm cao nhất có thể đạt được cho nhóm đầu tiên bằng tổng trừ đi phần tử tối thiểu của nhóm đó. 
3. Lặp lại quy trình tương tự cho nhóm thứ hai một cách độc lập. Hai nhóm không tương tác nên không thực hiện so sánh giữa các nhóm trong quá trình tính toán. 
4. So sánh hai kết quả và đưa ra kết quả lớn hơn. 

### Tại sao nó hoạt động 

Trong một nhóm, mọi hành động hợp lệ đều tương ứng với việc chọn chính xác một phần tử để loại bỏ. Mỗi hành động như vậy tạo ra kết quả bằng tổng trừ đi phần tử đó. Vì phép trừ là đơn điệu nên phần tử nhỏ nhất tạo ra tổng còn lại lớn nhất. Do đó, mức tối ưu cho mỗi nhóm được xác định duy nhất bởi phần tử tối thiểu. Tối ưu toàn cục là cực đại của hai nhóm tối ưu độc lập vì Darcy phải chọn đúng một nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = list(map(int, input().split()))
b = list(map(int, input().split()))

sum_a = sum(a)
sum_b = sum(b)

best_a = sum_a - min(a)
best_b = sum_b - min(b)

print(max(best_a, best_b))
```Mã đọc cả hai nhóm, tính tổng và giá trị cực tiểu của chúng, sau đó lấy điểm cao nhất có thể đạt được cho mỗi nhóm bằng cách trừ đi phần tử nhỏ nhất. Đầu ra cuối cùng đơn giản là giá trị lớn hơn trong hai giá trị được tính toán. 

Một lỗi phổ biến là tính lại tổng trong vòng lặp cho từng ứng cử viên bị loại, nhưng ở đây chúng tôi tránh điều đó hoàn toàn bằng cách tách tổng hợp (tổng và tối thiểu) khỏi việc ra quyết định (trừ tối thiểu). 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 5 5 5
3 3 2 2 2
```Đối với nhóm thứ nhất, tổng là 21 và tối thiểu là 1 nên điểm cao nhất là 20. Đối với nhóm thứ hai, tổng là 12 và tối thiểu là 2 nên điểm cao nhất là 10. 

| Nhóm | Tổng hợp | Tối thiểu | Điểm tốt nhất | 
| --- | --- | --- | --- | 
| A | 21 | 1 | 20 | 
| B | 12 | 2 | 10 | 

Đầu ra là 20 vì nhóm đầu tiên tốt hơn. 

Điều này xác nhận tính bất biến rằng chỉ phần tử nhỏ nhất có thể tháo rời được mới quan trọng. 

### Ví dụ 2 

đầu vào:```
10 10 10 10 1
7 8 9 10 6
```Đối với nhóm thứ nhất, tổng là 41 và min là 1, cho 40. Đối với nhóm thứ hai, tổng là 40 và min là 6, cho 34. 

| Nhóm | Tổng hợp | Tối thiểu | Điểm tốt nhất | 
| --- | --- | --- | --- | 
| A | 41 | 1 | 40 | 
| B | 40 | 6 | 34 | 

Đầu ra là 40, xác nhận rằng ngay cả nhóm thứ hai có tổng cao cũng không thể đánh bại nhóm đầu tiên sau khi loại bỏ tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các mảng có kích thước cố định có độ dài 5 mới được xử lý, mỗi mảng yêu cầu tổng thời gian không đổi và tính toán tối thiểu | 
| Không gian | O(1) | Chỉ một số lượng biến không đổi được lưu trữ | 

Giải pháp dễ dàng nằm trong giới hạn vì kích thước đầu vào không đổi. Ngay cả khi được chia tỷ lệ, nó sẽ vẫn tuyến tính theo quy mô nhóm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    sum_a = sum(a)
    sum_b = sum(b)

    best_a = sum_a - min(a)
    best_b = sum_b - min(b)

    return str(max(best_a, best_b))

# provided sample
assert run("5 1 5 5 5\n3 3 2 2 2\n") == "20"

# all equal
assert run("1 1 1 1 1\n2 2 2 2 2\n") == "8"

# second group better
assert run("1 1 1 1 10\n9 9 9 9 1\n") == "36"

# minimum edge case
assert run("1 2 3 4 5\n5 4 3 2 1\n") == "14"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 20 | tính đúng đắn cơ bản | 
| tất cả đều bình đẳng | 8 | phép trừ đối xứng và đúng | 
| sự thống trị hỗn hợp | 36 | chọn đúng nhóm | 
| thứ tự đảo ngược | 14 | lựa chọn tối thiểu nhất quán | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cả hai nhóm đều chứa các giá trị giống hệt nhau. Ví dụ: nếu cả hai nhóm đều`[3, 3, 3, 3, 3]`, tổng là 15 và loại bỏ bất kỳ phần tử nào sẽ mang lại 12. Thuật toán tính tổng 15 và tối thiểu 3 cho cả hai nhóm, tạo ra kết quả giống hệt nhau và trả về chính xác 12. 

Một trường hợp cạnh khác là khi phần tử tối thiểu là duy nhất và nhỏ hơn nhiều so với các phần tử khác. Vì`[100, 100, 100, 100, 1]`, thuật toán xác định chính xác rằng việc loại bỏ 1 mang lại 400, trong khi loại bỏ 100 bất kỳ mang lại 301. Bởi vì nó chỉ phụ thuộc vào mức tối thiểu nên không cần liệt kê và đảm bảo mức tối đa chính xác. 

Trường hợp cuối cùng là khi nhóm tốt nhất không phải là nhóm có tổng số tiền lớn nhất. Vì`[10, 10, 10, 10, 1]`so với`[9, 9, 9, 9, 9]`, tổng số lần lượt là 41 và 45, nhưng sau khi loại bỏ tối ưu, kết quả là 40 và 36. Thuật toán ưu tiên chính xác giá trị sau khi loại bỏ thay vì tổng thô.
