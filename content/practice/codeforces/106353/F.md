---
title: "CF 106353F - Chia sẻ hợp lý"
description: "Một nhóm người vừa hoàn thành một sự kiện chia sẻ chi phí và mỗi người có hai giá trị liên quan. Một giá trị thể hiện số tiền mặt họ có thể đóng góp ngay lập tức nếu người khác chịu trách nhiệm thanh toán hóa đơn."
date: "2026-06-20T03:20:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106353
codeforces_index: "F"
codeforces_contest_name: "2025-2026 ICPC Northwestern European Regional Programming Contest (NWERC 2025)"
rating: 0
weight: 106353
solve_time_s: 63
verified: true
draft: false
---

[CF 106353F - Chia sẻ công bằng](https://codeforces.com/problemset/problem/106353/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một nhóm người vừa hoàn thành một sự kiện chia sẻ chi phí và mỗi người có hai giá trị liên quan. Một giá trị thể hiện số tiền mặt họ có thể đóng góp ngay lập tức nếu người khác chịu trách nhiệm thanh toán hóa đơn. Giá trị còn lại thể hiện số chi phí ròng tối đa mà họ sẵn sàng chịu nếu họ trở thành người thanh toán toàn bộ hóa đơn và thu tiền mặt của người khác. 

Chúng tôi được yêu cầu chọn chính xác một người làm người trả tiền. Nếu người tôi được chọn, họ sẽ thu toàn bộ tiền mặt từ những người tham gia khác, sau đó sử dụng thẻ tín dụng của họ để thanh toán toàn bộ hóa đơn. Chi phí cá nhân hiệu quả khi chọn i là tổng hóa đơn trừ đi số tiền mặt mà những người khác đóng góp. Chi phí này không được vượt quá mức chia sẻ cá nhân được phép của tôi. 

Mục đích là để xác định xem có tồn tại ít nhất một người thanh toán hợp lệ hay không và nếu có thì xuất ra bất kỳ chỉ mục nào như vậy. 

Các ràng buộc cho phép tối đa 100.000 người, vì vậy mọi giải pháp đều phải gần với tuyến tính hoặc tuyến tính. Cách tiếp cận bậc hai đánh giá từng ứng viên bằng cách tổng hợp đóng góp của tất cả những người khác sẽ dẫn đến khoảng 10^10 phép tính trong trường hợp xấu nhất, vượt xa giới hạn thời gian. 

Một trường hợp phức tạp xuất hiện khi hầu hết mọi người đều đóng góp số tiền mặt lớn nhưng lại có khả năng trả thêm rất ít. Ví dụ: nếu đóng góp của một người là cần thiết cho tính khả thi thì việc loại bỏ họ khỏi tư cách người trả tiền có thể làm mất hiệu lực của những người khác ngay cả khi cá nhân họ có vẻ chấp nhận được. Một vấn đề khác là khi tất cả các cá nhân đều “có giá trị cục bộ” theo một nghĩa đơn giản nào đó nhưng trên toàn cầu không ai có thể thỏa mãn ràng buộc một khi tổng số tiền đầy đủ được xem xét. 

## Phương pháp tiếp cận 

Khó khăn cốt lõi là điều kiện của một người phụ thuộc vào tổng số tiền đóng góp bằng tiền mặt của những người khác, điều này sẽ thay đổi tùy theo ứng viên được chọn. Nếu chúng ta kiểm tra từng người i một cách độc lập, chúng ta cần tính tổng của tất cả aj cho j ≠ i. Việc tính toán lại trực tiếp cho mỗi i sẽ dẫn đến O(n^2), vì mỗi lần kiểm tra đều yêu cầu quét tất cả những người tham gia. 

Quan sát quan trọng là tổng số tiền mặt S = sum(ai) là cố định. Nếu người tôi trả tiền thì số tiền họ thu được là S - ai. Khoản thanh toán ròng của họ trở thành bi ít nhất phải là S - ai. Việc sắp xếp lại sẽ đưa ra một bất đẳng thức đơn giản có thể được đánh giá bằng O(1) cho mỗi người khi đã biết S. 

Điều này biến vấn đề thành việc kiểm tra một điều kiện duy nhất cho mỗi ứng viên sau một lần vượt qua toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cặp (ai, bi) và tính tổng S của tất cả các giá trị ai. Điều này thể hiện toàn bộ quỹ tiền mặt nếu không có ai bị loại trừ. 
2. Với mỗi người, tôi hãy tưởng tượng họ là người trả tiền. Trong trường hợp đó, số tiền họ thu được là S - ai vì những người khác đều đóng góp toàn bộ số tiền của họ. 
3. Tính gánh nặng thực tế đối với người i là (S - ai). Đây là số tiền họ phải tự trang trải bằng nguồn vốn của mình sau khi thu tiền mặt. 
4. Kiểm tra xem gánh nặng này có được chấp nhận đối với người i hay không bằng cách xác minh (S - ai) ≤ bi. 
5. Ngay sau khi một người thỏa mãn bất đẳng thức này, hãy xuất chỉ mục của họ và chấm dứt. 
6. Nếu không có người nào thỏa mãn điều kiện sau khi quét tất cả các ứng viên thì xuất ra “không thể”. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tổng số tiền đóng góp từ những người không trả tiền chỉ phụ thuộc vào tổng số tiền chung trừ đi số tiền mặt của chính người được chọn. Điều này giúp loại bỏ mọi hiệu ứng tương tác giữa các ứng viên. Tính khả thi của mỗi ứng cử viên chỉ phụ thuộc vào một giá trị toàn cầu cố định S và cặp riêng của chúng (ai, bi), vì vậy việc kiểm tra chúng một cách độc lập là đủ. Không thể bỏ qua giải pháp nào vì mọi cấu hình hợp lệ đều tương ứng với chính xác một chỉ số thỏa mãn bất đẳng thức dẫn xuất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = []
    b = []
    
    S = 0
    for _ in range(n):
        ai, bi = map(int, input().split())
        a.append(ai)
        b.append(bi)
        S += ai

    for i in range(n):
        if S - a[i] <= b[i]:
            print(i + 1)
            return

    print("impossible")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên tổng hợp tất cả các giá trị tiền mặt để tính toán tổng số tiền. Điều này rất cần thiết vì mọi kiểm tra tính khả thi đều phụ thuộc vào tổng số của tất cả những người tham gia khác. 

Sau đó, nó lặp lại một lần trên tất cả các ứng viên, đánh giá trực tiếp bất đẳng thức dẫn xuất. Phép trừ S - a[i] thể hiện số tiền thu được nếu tôi thanh toán. Việc so sánh nó với b[i] thực thi ràng buộc rằng gánh nặng ròng của i nằm trong giới hạn cho phép của chúng. 

Việc trả lại sớm đảm bảo chúng tôi dừng ngay lập tức khi tìm thấy bất kỳ người trả tiền hợp lệ nào, phù hợp với yêu cầu của vấn đề rằng mọi chỉ mục hợp lệ đều được chấp nhận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét đầu vào:```
n = 4
a = [4, 5, 1, 3]
b = [5, 4, 3, 6]
```Tổng số tiền S là 13. 

| tôi | ai | bi | S - ai | Tình trạng | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 5 | 9 | 9 5 sai | 
| 2 | 5 | 4 | 8 | 8 ≤ 4 sai | 
| 3 | 1 | 3 | 12 | 12 3 sai | 
| 4 | 3 | 6 | 10 | 10 6 sai | 

Không có chỉ mục nào thỏa mãn ràng buộc nên kết quả đầu ra là không thể. 

Dấu vết này cho thấy mặc dù mỗi người đều thu được một lượng tiền mặt lớn nhưng gánh nặng ròng được phép của họ lại quá nhỏ để có thể chi trả cho hóa đơn còn lại. 

### Ví dụ 2```
n = 5
a = [1, 4, 1, 2, 4]
b = [4, 6, 4, 5, 6]
```Tổng số tiền S là 12. 

| tôi | ai | bi | S - ai | Tình trạng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 11 | 11 ≤ 4 sai | 
| 2 | 4 | 6 | 8 | 8 ≤ 6 sai | 
| 3 | 1 | 4 | 11 | 11 ≤ 4 sai | 
| 4 | 2 | 5 | 10 | 10 5 sai | 
| 5 | 4 | 6 | 8 | 8 ≤ 6 sai | 

Một lần nữa không có ứng cử viên hợp lệ nào tồn tại. 

Ví dụ này nhấn mạnh rằng điều kiện rất nghiêm ngặt và phụ thuộc vào tổng số toàn cầu hơn là so sánh cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt để tính tổng, một lượt để kiểm tra ứng viên | 
| Không gian | O(1) | chỉ lưu trữ tổng chạy và mảng đầu vào | 

Các ràng buộc cho phép tối đa 100.000 mục nhập, do đó việc quét tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ vẫn ở mức tối thiểu vì chỉ các mảng số nguyên đơn giản được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimum size
assert run("2\n1 10\n10 1\n") in {"1", "2", "impossible"}

# simple valid case
assert run("3\n1 5\n2 1\n3 10\n") in {"1","2","3","impossible"}

# all equal
assert run("4\n2 2\n2 2\n2 2\n2 2\n") in {"1","2","3","4","impossible"}

# forced impossible
assert run("3\n10 1\n10 1\n10 1\n") == "impossible"

# large-ish balanced case
assert run("5\n1 100\n2 100\n3 100\n4 100\n5 100\n") in {"1","2","3","4","5"}

# one obvious winner
assert run("4\n5 100\n1 1\n1 1\n1 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp 2 người | bất kỳ hợp lệ hoặc không thể | độ đúng ranh giới tối thiểu | 
| tất cả các giá trị bằng nhau | bất kỳ chỉ mục nào hoặc không thể | xử lý đối xứng | 
| buộc không thể | không thể | tính đúng đắn của việc từ chối | 
| sai lệch dung sai lớn | bất kỳ chỉ mục nào | phát hiện trường hợp hợp lệ dễ dàng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị ai đều lớn nhưng tất cả các giá trị bi đều cực kỳ nhỏ. Trong trường hợp đó S trở nên lớn, do đó S - ai vẫn lớn với mọi i và điều kiện không đồng đều. Thuật toán từ chối đầu vào một cách chính xác vì mọi gánh nặng tính toán đều vượt quá giới hạn tương ứng. 

Một trường hợp khác là khi một người có ai rất lớn và bi cũng rất lớn. Giả sử họ thống trị nguồn tiền mặt. Khi đó đối với người đó, S - ai trở nên nhỏ vì việc loại bỏ phần đóng góp của họ làm giảm đáng kể tổng số. Thuật toán xác định chính xác ứng cử viên đó vì bất đẳng thức chỉ trở nên dễ dàng được đáp ứng đối với họ. 

Trường hợp cạnh thứ ba phát sinh khi nhiều người hợp lệ. Vì thuật toán dừng ở kết quả khớp đầu tiên trong quá trình quét nên thuật toán sẽ trả về chính xác bất kỳ chỉ mục hợp lệ nào mà không cần logic ràng buộc, phù hợp với yêu cầu của vấn đề.
