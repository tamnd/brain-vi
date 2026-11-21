---
title: "CF 2192C - Súng đa năng"
description: "Chúng ta được đưa ra một kịch bản trò chơi bắn súng trong đó chúng ta có một khẩu súng với băng đạn gồm n viên đạn. Mỗi viên đạn có một sát thương cố định ai và các viên đạn được bắn theo trình tự, mỗi viên một giây."
date: "2026-06-07T20:56:23+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2192
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1081 (Div. 2)"
rating: 1300
weight: 2192
solve_time_s: 125
verified: true
draft: false
---

[CF 2192C - Súng đa năng](https://codeforces.com/problemset/problem/2192/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tham lam, toán học 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một kịch bản trò chơi bắn súng trong đó chúng ta có một khẩu súng với băng đạn`n`đạn. Mỗi viên đạn có sát thương cố định`a_i`, và các viên đạn được bắn theo trình tự, mỗi giây một viên. Tạp chí phải được làm trống hoàn toàn trước khi tải lại`k`giây có thể xảy ra và sau khi nạp lại, chuỗi đạn tương tự sẽ được khôi phục. Kẻ thù bắt đầu bằng`h`sức khỏe và mục tiêu của chúng tôi là đưa nó về 0 hoặc thấp hơn càng nhanh càng tốt. Ngoài ra, chúng ta có thể tùy ý hoán đổi hai viên đạn bất kỳ trong băng đạn một lần trước trận chiến để tối ưu hóa lượng sát thương gây ra. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm, mỗi trường hợp mô tả số lượng đạn, sức khỏe của kẻ thù, thời gian nạp đạn và danh sách sát thương của đạn. Đầu ra là số giây tối thiểu cần thiết để tiêu diệt kẻ thù trong mỗi trường hợp thử nghiệm. 

Các ràng buộc rất lớn:`n`có thể lên tới 2×10^5, và`h`Và`k`lên đến 10^9. Vì tổng của`n`trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi 2×10^5, chúng ta phải thiết kế một thuật toán chạy theo thời gian tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. Các phương pháp ngây thơ mô phỏng từng giây bắn sẽ thất bại, đặc biệt đối với các trường hợp có tốc độ cao.`h`giá trị, bởi vì điều đó có thể liên quan đến hàng tỷ hoạt động. 

Các trường hợp có lợi thế không rõ ràng bao gồm các tình huống trong đó viên đạn đầu tiên quá yếu và sự hoán đổi tối ưu liên quan đến việc di chuyển một viên đạn mạnh về phía trước. Ví dụ, nếu`n = 5`,`h = 10`,`k = 2`, Và`a = [1, 1, 1, 10, 1]`, một cách tiếp cận ngây thơ mà không hoán đổi có thể mô phỏng ba viên đạn`1`và sau đó tải lại, mất nhiều thời gian không cần thiết. Nước đi đúng là đổi viên đạn cuối cùng`10`vào vị trí sớm hơn để giảm thiểu thời gian tiêu diệt. 

Một trường hợp khác xảy ra khi tổng số viên đạn trong một băng đạn nhỏ hơn`h`, yêu cầu tải lại đầy đủ nhiều lần. Phải cẩn thận để tính toán chính xác số chu kỳ đầy đủ và số đạn còn lại cần thiết, không chỉ phân chia tầng, vì việc đánh giá quá cao số lần nạp lại có thể làm tăng thời gian tiêu diệt. 

## Phương pháp tiếp cận 

Phương pháp vũ phu sẽ là mô phỏng từng giây bắn, áp dụng hoán đổi tùy chọn ở mọi cặp vị trí có thể và kiểm tra tổng thời gian cần thiết. Đối với một trường hợp thử nghiệm duy nhất, điều này có thể liên quan đến các hoán đổi O(n^2) và với mỗi lần hoán đổi, chúng ta có thể cần mô phỏng tối đa`h`giây. Điều này rõ ràng là không thể thực hiện được với`n`lên đến 2×10^5 và`h`lên tới 10^9, vì số lượng thao tác có thể đạt tới 10^14. 

Quan sát quan trọng là sự hoán đổi có ý nghĩa duy nhất là đưa sát thương của viên đạn lớn nhất về phía trước ở vị trí đầu tiên, bởi vì những viên đạn đầu tiên luôn được bắn trước và do đó có tác động lớn nhất đến thời gian tiêu diệt. Khi biết được điều này, chúng ta có thể xem xét hai tình huống: trình tự ban đầu và trình tự có viên đạn đầu tiên được hoán đổi bằng viên đạn có lực sát thương tối đa. Trong cả hai trường hợp, chúng ta có thể tính số lượng đạn tối thiểu cần thiết để đạt được`h`sức khỏe và sau đó chuyển nó thành thời gian bằng cách tính đến số lần tải lại toàn bộ và một phần chu kỳ. 

Bằng cách tính toán trước tổng tiền tố của các viên đạn theo trình tự, chúng ta có thể nhanh chóng xác định cần bao nhiêu viên đạn mà không cần mô phỏng từng giây riêng lẻ. Điều này làm giảm vấn đề từ O(n^2) xuống O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu với tất cả các giao dịch hoán đổi | O(n^2 + h) | O(n) | Quá chậm | 
| Hoán đổi được tối ưu hóa + Tổng tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`h`,`k`, và mảng sát thương của đạn`a`. 
2. Xác định chỉ số sát thương tối đa của viên đạn trong`a`. Hãy cân nhắc việc hoán đổi nó với dấu đầu dòng đầu tiên để tạo một chuỗi được tối ưu hóa tùy chọn. Tạo nên`a_swapped`như trình tự sau sự hoán đổi tùy chọn này. 
3. Tính tổng tiền tố cho cả chuỗi gốc và chuỗi hoán đổi. Tổng tiền tố`prefix[i]`đại diện cho tổng thiệt hại gây ra bằng cách bắn phát đầu tiên`i`viên đạn theo thứ tự. 
4. Với mỗi chuỗi, hãy xác định số lượng đạn tối thiểu cần thiết để giảm`h`bằng 0 hoặc thấp hơn. Nếu tổng số viên đạn trong băng đạn là`s`, sau đó có thể áp dụng tạp chí đầy đủ`full_cycles = h // s`, phần còn lại được xử lý bằng các viên đạn bổ sung từ băng đạn tiếp theo. 
5. Chuyển đổi số lượng đạn cần thiết thành tổng thời gian. Mỗi tạp chí đầy đủ tiêu thụ`n`giây, cộng với việc tải lại`k`giây. Các tạp chí một phần sẽ thêm số lượng đạn tương ứng trong vài giây mà không cần tải lại nếu chúng không hết băng đạn. 
6. So sánh tổng thời gian của chuỗi ban đầu và chuỗi được hoán đổi. Xuất ra thời gian nhỏ hơn. 

Tại sao nó hoạt động: Những viên đạn đầu tiên trong băng đạn đóng góp nhiều nhất vào việc giảm thời gian tiêu diệt vì chúng được bắn trước khi nạp đạn. Việc hoán đổi bất kỳ cặp đạn nào khác không cải thiện tổng tiền tố của các lần bắn ban đầu đủ để giảm thêm thời gian tối thiểu. Tính toán thiệt hại bằng cách sử dụng tổng tiền tố và chu kỳ đảm bảo chúng tôi tính toán chính xác tất cả các lần tải lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_time_to_kill(n, h, k, a):
    max_idx = max(range(n), key=lambda i: a[i])
    if max_idx != 0:
        a_swapped = a[:]
        a_swapped[0], a_swapped[max_idx] = a_swapped[max_idx], a_swapped[0]
    else:
        a_swapped = a[:]
    
    def compute_time(arr):
        prefix = [0] * n
        prefix[0] = arr[0]
        for i in range(1, n):
            prefix[i] = prefix[i-1] + arr[i]
        total = prefix[-1]
        if total >= h:
            for i, val in enumerate(prefix):
                if val >= h:
                    return i + 1
        else:
            full_cycles = h // total
            rem = h % total
            time = full_cycles * (n + k)
            if rem == 0:
                return time - k
            for i, val in enumerate(prefix):
                if val >= rem:
                    return time + i + 1
        return None

    return min(compute_time(a), compute_time(a_swapped))

t = int(input())
for _ in range(t):
    n, h, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(min_time_to_kill(n, h, k, a))
```chức năng`min_time_to_kill`tính toán số giây tối thiểu cho một trường hợp thử nghiệm. Đầu tiên chúng ta tùy ý hoán đổi viên đạn đầu tiên với viên đạn có sát thương tối đa. các`compute_time`hàm này sử dụng tổng tiền tố để xác định số lượng đạn cần thiết và thời gian nạp lại toàn bộ băng đạn được bao gồm trong tổng thời gian. các`min`của trình tự ban đầu và trình tự được hoán đổi đảm bảo chúng tôi tính đến sự hoán đổi tùy chọn. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`n = 5, h = 10, k = 1, a = [4, 2, 3, 5, 3]`| Bước | Chuỗi đạn | Tiền tố Tổng | Đạn Cần Thiết | Thời gian | 
| --- | --- | --- | --- | --- | 
| Bản gốc | [4,2,3,5,3] | [4,6,9,14,17] | 4,6,9 ≥ 10 → 3 viên | 3 giây | 
| Hoán đổi thứ 2 và tối đa(5) → [5,2,3,4,3] | [5,7,10,14,17] | 5,7,10 ≥ 10 → 3 viên | 3 giây | | 

Thời gian = 3 giây 

**Ví dụ 2** 

đầu vào:`n = 3, h = 10, k = 2, a = [1, 2, 3]`| Bước | Chuỗi đạn | Tiền tố Tổng | Đạn Cần Thiết | Thời gian | 
| --- | --- | --- | --- | --- | 
| Bản gốc | [1,2,3] | [1,3,6] | Tổng tạp chí đầy đủ 6 < 10, full_cycles = 1 | Thời gian = 3 + 2 lần nạp đạn + 1,2 viên → tổng 7 | 
| Hoán đổi đầu tiên và tối đa → [3,2,1] | [3,5,6] | 6 < 10 → đầy đủ chu kỳ 1 | Cần 4 sát thương còn lại → 3+2 = 5 → tổng 7 | | 

Thời gian = 7 giây 

Những ví dụ này cho thấy tổng tiền tố và hoán đổi tùy chọn xác định chính xác thời gian tối thiểu như thế nào, bao gồm cả tính toán tải lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tìm chỉ mục tối đa, tính tổng tiền tố và kiểm tra các dấu đầu dòng tối thiểu tất cả đều tuyến tính`n`| 
| Không gian | O(n) | Mảng tổng tiền tố và bản sao hoán đổi của mảng | 

Với tổng số`n`trên tất cả các trường hợp thử nghiệm 2×10^5, tổng số thao tác đều nằm trong khoảng 2×10^5 × 2 = 4×10^5, vừa vặn thoải mái trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("6\n5 10 1\n4 2 3 5 3\n5 10 1\n4 2 3 7 3\n3 10
```
