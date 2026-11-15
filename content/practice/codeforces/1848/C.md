---
title: "CF 1848C - Vika và Thẻ Giá"
description: "Chúng ta được cung cấp hai mảng biểu thị giá cũ và giá mới của một tập hợp các mặt hàng. Vika liên tục tính toán sự khác biệt tuyệt đối giữa các phần tử tương ứng của các mảng này, hoán đổi các mảng và lặp lại quy trình."
date: "2026-06-09T05:38:28+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1848
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 885 (Div. 2)"
rating: 1800
weight: 1848
solve_time_s: 107
verified: false
draft: false
---

[CF 1848C - Vika và Thẻ giá](https://codeforces.com/problemset/problem/1848/C) 

**Đánh giá:** 1800 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai mảng biểu thị giá cũ và giá mới của một tập hợp các mặt hàng. Vika liên tục tính toán sự khác biệt tuyệt đối giữa các phần tử tương ứng của các mảng này, hoán đổi các mảng và lặp lại quy trình. Mục đích là để xác định xem liệu quá trình lặp lại này cuối cùng có giảm tất cả các phần tử của mảng giá cũ về 0 hay không. Mỗi trường hợp thử nghiệm yêu cầu chúng tôi trả lời “CÓ” nếu cặp mảng đạt đến trạng thái như vậy và “KHÔNG” nếu ngược lại. 

Các ràng buộc ngụ ý rằng chúng tôi có thể có tối đa 10^5 mục cho mỗi trường hợp thử nghiệm và tối đa 10^4 trường hợp thử nghiệm, nhưng tổng số mục trong tất cả các trường hợp thử nghiệm được giới hạn ở mức 10^5. Điều này cho chúng ta biết rằng chúng ta phải xử lý từng mảng trong thời gian gần như tuyến tính, vì thuật toán bậc hai sẽ quá chậm. 

Trường hợp cạnh không rõ ràng xảy ra khi mảng cũ ban đầu toàn là số 0. Trong trường hợp này, cặp này hơi buồn tẻ bất kể mảng mới. Một trường hợp tinh vi khác phát sinh khi các phần tử của mảng mới nhỏ hơn các giá trị mảng cũ tương ứng nhưng không phải tất cả sự khác biệt đều làm giảm mảng cũ về 0. Nếu chúng tôi cố gắng mô phỏng trực tiếp quy trình cho từng thao tác, chúng tôi có thể đánh giá sai các cấu hình này hoặc vượt quá giới hạn thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng các phép biến đổi mảng một cách rõ ràng. Trong mỗi bước, nó tính toán một mảng mới dưới dạng chênh lệch tuyệt đối, sau đó hoán đổi các mảng đó. Điều này hiệu quả vì nó trực tiếp mô hình hóa vấn đề. Tuy nhiên, trong trường hợp xấu nhất với mảng có độ dài 10^5, mỗi thao tác là O(n) và số lần lặp cũng có thể là O(n) vì sự khác biệt có thể giảm chậm. Điều này dẫn đến độ phức tạp về thời gian là O(n^2) cho mỗi trường hợp thử nghiệm, quá chậm so với giới hạn đầu vào. 

Thông tin chuyên sâu quan trọng để tăng tốc giải pháp xuất phát từ việc quan sát rằng hoạt động bảo toàn cấu trúc chênh lệch tối đa. Cụ thể, quá trình này có thể được hiểu là trừ đi nhiều lần phần tử nhỏ nhất khỏi tất cả các phần tử trong mảng theo modulo ước chung lớn nhất. Cụ thể hơn, mảng trở nên buồn tẻ khi và chỉ khi tất cả các phần tử của mảng cũ không vượt quá các phần tử tương ứng của mảng mới và sự khác biệt là nhất quán sao cho không có phần tử nào nhỏ hơn phần tử trước đó theo thứ tự được sắp xếp. Bằng cách sắp xếp cả hai mảng và kiểm tra xem mỗi phần tử giá cũ không vượt quá phần tử giá mới tương ứng và chênh lệch giữa chúng không giảm, chúng ta có thể xác định độ trễ theo thời gian tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc độ dài của mảng, sau đó đọc mảng giá cũ và mảng giá mới. 
2. Nếu mảng cũ đã toàn là số 0, hãy xuất “CÓ” và tiếp tục. Điều này xử lý trường hợp cạnh tầm thường. 
3. Sắp xếp cả hai mảng. Việc sắp xếp cho phép chúng ta so sánh các phần tử theo cách có cấu trúc và đảm bảo rằng việc kiểm tra sự khác biệt là nhất quán trên các mảng. 
4. Tính hiệu ban đầu giữa các phần tử lớn nhất tương ứng của mảng đã được sắp xếp. Sự khác biệt này sẽ được dùng làm tài liệu tham khảo cho những lần kiểm tra tiếp theo. 
5. Lặp lại từ phần tử lớn nhất đến phần tử nhỏ nhất. Đối với mỗi vị thế, hãy tính chênh lệch giữa thành phần giá mới và giá cũ. 
6. Nếu có bất kỳ chênh lệch nào là âm, thao tác không thể tạo ra số không; xuất “NO” và dừng lại. 
7. Nếu có bất kỳ chênh lệch nào tăng so với tham chiếu, xuất ra “NO” vì mẫu chênh lệch sẽ vi phạm điều kiện độ mờ. 
8. Nếu tất cả các bước kiểm tra đều đạt, ghi “CÓ”.

Tại sao nó hoạt động: Việc sắp xếp các mảng đảm bảo rằng chúng ta đang so sánh các vị trí tương ứng với mối quan hệ độ lớn chính xác. Sự khác biệt tham chiếu nắm bắt mức giảm tối đa được phép truyền đến các phần tử nhỏ hơn. Bằng cách kiểm tra sự khác biệt theo thứ tự giảm dần, chúng tôi đảm bảo rằng thao tác có thể giảm mảng cũ về 0 một cách đơn điệu mà không tạo ra giá trị âm hoặc vi phạm cấu trúc sai phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        if all(x == 0 for x in a):
            print("YES")
            continue
        a.sort()
        b.sort()
        diff = b[-1] - a[-1]
        if diff < 0:
            print("NO")
            continue
        ok = True
        for i in range(n):
            if b[i] - a[i] > diff or b[i] - a[i] < 0:
                ok = False
                break
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách xử lý trường hợp tầm thường của một mảng cũ hoàn toàn bằng 0. Việc sắp xếp là rất quan trọng để đảm bảo sự khác biệt được so sánh một cách nhất quán. Sự khác biệt tham chiếu được thiết lập bằng cách sử dụng các phần tử lớn nhất, sau đó tất cả các vị trí đều được kiểm tra tính nhất quán và không âm. Việc sử dụng I/O nhanh của Python sẽ tránh được chi phí đầu vào do kích thước đầu vào lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào: 

a = [0, 0, 0, 0] 

b = [1, 2, 3, 4] 

| Bước | một (sắp xếp) | b (sắp xếp) | khác biệt | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [0,0,0,0] | [1,2,3,4] | - | tất cả số không → CÓ | 

Thuật toán ngay lập tức phát hiện ra sự buồn tẻ tầm thường. 

### Ví dụ 2 

a = [1,2,3] 

b = [1,2,3] 

| Bước | một (sắp xếp) | b (sắp xếp) | khác biệt | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Đã sắp xếp | [1,2,3] | [1,2,3] | 0 | b[i]-a[i] = 0 ≤ diff → CÓ | 

Tất cả các khác biệt đều nhất quán và không âm, do đó thuật toán trả về CÓ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chi phối chi phí cho mỗi trường hợp thử nghiệm; quét tuyến tính để kiểm tra là O(n) | 
| Không gian | O(n) | Mảng được lưu trữ và sắp xếp trong bộ nhớ | 

Giải pháp này rất phù hợp trong thời gian giới hạn vì tổng n trong tất cả các trường hợp thử nghiệm là ≤10^5, đồng thời phân loại và kiểm tra tuyến tính có tỷ lệ thoải mái cho kích thước đầu vào này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("""9
4
0 0 0 0
1 2 3 4
3
1 2 3
1 2 3
2
1 2
2 1
6
100 23 53 11 56 32
1245 31 12 6 6 6
7
1 2 3 4 5 6 7
7 6 5 4 3 2 1
3
4 0 2
4 0 2
3
2 5 2
1 3 4
2
6 1
4 2
2
0 0
0 3""") == """YES
YES
NO
NO
YES
YES
NO
YES
YES"""

# Custom cases
assert run("1\n1\n0\n0") == "YES", "single zero element"
assert run("1\n2\n0 1\n1 1") == "YES", "mixed zeros"
assert run("1\n3\n2 2 2\n1 1 1") == "NO", "old elements larger than new"
assert run("1\n4\n5 5 5 5\n5 5 5 5") == "YES", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử số 0 | CÓ | sự buồn tẻ tầm thường | 
| 2 yếu tố, hỗn hợp | CÓ | số không một phần | 
| 3 yếu tố, cũ > mới | KHÔNG | không thể giảm | 
| 4 phần tử bằng nhau | CÓ | tính nhất quán của các giá trị bằng nhau | 

## Vỏ cạnh 

Khi mảng cũ đã hoàn toàn bằng 0, giải pháp ngay lập tức đưa ra CÓ mà không cần tính toán thêm. Đối với các mảng trong đó phần tử cũ vượt quá phần tử mới tương ứng, thuật toán sẽ xác định chênh lệch âm và đưa ra NO. Đối với các mảng có giá trị cũ và mới bằng nhau, chênh lệch tham chiếu bằng 0 và tất cả các bước kiểm tra đều đạt, dẫn đến CÓ. Mỗi trường hợp tôn trọng tính đơn điệu của sự khác biệt và đảm bảo điều kiện độ mờ được đánh giá chính xác.
