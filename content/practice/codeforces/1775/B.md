---
title: "CF 1775B - Người làm vườn và mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi số nguyên được chỉ định bởi tập hợp các bit 1 thay vì giá trị số của nó. Nhiệm vụ là xác định xem có tồn tại hai dãy con riêng biệt của mảng tạo ra cùng một OR theo bit hay không."
date: "2026-06-09T11:53:11+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1775
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 843 (Div. 2)"
rating: 1300
weight: 1775
solve_time_s: 111
verified: true
draft: false
---

[CF 1775B - Người làm vườn và dàn trận](https://codeforces.com/problemset/problem/1775/B) 

**Đánh giá:** 1300 
**Tas:** bitmask, thuật toán xây dựng 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi số nguyên được chỉ định bởi tập hợp các bit 1 thay vì giá trị số của nó. Nhiệm vụ là xác định xem có tồn tại hai dãy con riêng biệt của mảng tạo ra cùng một OR theo bit hay không. Một dãy con được coi là khác biệt nếu các tập hợp chỉ số được chọn khác nhau, không nhất thiết nếu các giá trị số khác nhau. Bitwise OR của một chuỗi là sự kết hợp của tất cả các bit được đặt trong bất kỳ phần tử nào. 

Đầu vào cho phép lên tới$10^5$trường hợp thử nghiệm và tổng số$10^5$đặt bit trên tất cả các số trong tất cả các bài kiểm tra. Điều này ngụ ý rằng tổng số phần tử mảng trên mỗi thử nghiệm bị giới hạn một cách hiệu quả, nhưng chúng ta vẫn phải xem xét các mảng riêng lẻ theo chiều dài$10^5$. Bất kỳ thuật toán nào có thời gian tuyến tính hoặc gần tuyến tính kém hơn cho mỗi lần kiểm tra sẽ quá chậm vì tổng số thao tác sẽ vượt quá$10^8$ĐẾN$10^9$. Một cách tiếp cận ngây thơ liệt kê tất cả các chuỗi con sẽ chậm theo cấp số nhân và ngay lập tức không khả thi. 

Các trường hợp cạnh không rõ ràng xảy ra khi các số có các bit chồng chéo cho phép một tập hợp con tạo ra OR giống như một tập hợp lớn hơn hoặc khi các số riêng lẻ là tập hợp con của các số khác. Ví dụ, hãy xem xét một mảng$[1, 3]$, Ở đâu$1 = 0b1$Và$3 = 0b11$. Dãy con chỉ gồm phần tử đầu tiên có OR$1$, trong khi dãy con bao gồm cả hai có OR$3$. Nếu chúng ta có$[3, 3]$, chỉ lấy phần tử đầu tiên hoặc phần tử thứ hai sẽ mang lại OR$3$, tạo ra một câu trả lời hợp lệ. Một thuật toán ngây thơ chỉ so sánh các giá trị chứ không phải chỉ số sẽ kết luận sai rằng không có dãy con trùng lặp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các chuỗi con và tính toán OR của chúng. Đối với một mảng có độ dài$n$, có$2^n$những hậu quả có thể xảy ra, làm cho cách tiếp cận này hoàn toàn không thực tế ngay cả đối với$n$nhỏ như$20$. Ngay cả việc lưu trữ tất cả các kết quả OR cũng sẽ tiêu tốn bộ nhớ theo cấp số nhân, vì vậy điều này là không khả thi. 

Cái nhìn sâu sắc quan trọng là giảm bớt vấn đề để phát hiện các giá trị OR lặp lại theo cách năng động, tăng dần. Chúng tôi nhận thấy rằng thao tác OR là đơn điệu: việc thêm các phần tử vào một chuỗi con không thể tắt bất kỳ bit nào đã được đặt. Do đó, nếu chúng tôi theo dõi tất cả các OR có thể được hình thành kết thúc ở mỗi phần tử, chúng tôi có thể cập nhật tập hợp các OR có thể đạt được một cách hiệu quả bằng cách lặp qua mảng. Mỗi phần tử mới mở rộng OR hiện có bằng cách OR-ing với phần tử hiện tại. Điều quan trọng là nếu bất kỳ OR nào được tạo ra nhiều lần, chúng ta đã tìm thấy hai dãy con riêng biệt. Quan sát này cho phép chúng tôi giảm vấn đề thành phép lặp dựa trên tập hợp với thuộc tính là giá trị OR chỉ tăng và không bao giờ vượt quá độ dài bit của các số, giữ cho tập hợp OR được theo dõi có thể quản lý được. Tổng số giá trị OR duy nhất cho số nguyên lên tới$2 \cdot 10^5$được giới hạn bởi số lượng bit, xung quanh$18$hoặc$19$, đảm bảo hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| HOẶC Đặt tuyên truyền | O(n * B) | O(B) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một tập hợp trống có tên`seen`để lưu trữ tất cả các giá trị OR gặp phải trong bất kỳ chuỗi con nào. 
2. Lặp lại các phần tử của mảng theo thứ tự. Đối với mỗi phần tử, hãy duy trì một bộ`current`của OR có thể được hình thành bằng cách sử dụng các chuỗi con kết thúc tại phần tử này. Bắt đầu`current`với giá trị của chính phần tử đó. 
3. Với mỗi giá trị OR`val`trong tập lặp trước đó, tính toán`val | element`và thêm nó vào`current`. Điều này giải thích cho việc mở rộng tất cả các chuỗi con trước đó với phần tử hiện tại. 
4. Sau khi tính toán`current`, kiểm tra từng giá trị trong`current`chống lại`seen`. Nếu bất kỳ giá trị nào đã có trong`seen`, chúng ta có hai dãy con khác nhau tạo ra cùng một OR, vì vậy câu trả lời là "Có". 
5. Nếu không, hãy cộng tất cả các giá trị trong`current`ĐẾN`seen`và tiếp tục lặp lại. 
6. Nếu vòng lặp kết thúc mà không tìm thấy OR trùng lặp, câu trả lời là "Không". 

Tính bất biến đó là`seen`luôn chứa tất cả các giá trị OR từ tất cả các chuỗi con được xem xét cho đến nay. Vì OR là đơn điệu nên chúng ta chỉ thêm OR mới. Bất kỳ sự trùng lặp nào trong`seen`tương ứng với hai dãy con khác nhau vì quá trình mở rộng đảm bảo có ít nhất một chỉ mục khác nhau giữa các dãy con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = []
        for _ in range(n):
            parts = list(map(int, input().split()))
            k, bits = parts[0], parts[1:]
            val = 0
            for b in bits:
                val |= 1 << (b-1)
            arr.append(val)
        seen = set()
        prev = set()
        found = False
        for num in arr:
            current = {num}
            for val in prev:
                current.add(val | num)
            for val in current:
                if val in seen:
                    print("Yes")
                    found = True
                    break
            if found:
                break
            seen.update(current)
            prev = current
        if not found:
            print("No")

solve()
```Giải pháp xây dựng các giá trị số nguyên từ biểu diễn đầu vào bằng cách sử dụng dịch chuyển bit. Mỗi phần tử được xử lý một lần. các`prev`set lưu trữ tất cả OR kết thúc ở phần tử trước đó, được mở rộng bằng OR-ing với số hiện tại. Chúng tôi ngay lập tức phát hiện các OR lặp lại bằng cách sử dụng`seen`. Việc sử dụng các bộ đảm bảo rằng các bản sao được xử lý hiệu quả và ngăn chặn việc tính toán lại không cần thiết. Theo dõi`prev`riêng biệt đảm bảo rằng chỉ các phần mở rộng liên tiếp mới được xem xét, tránh bỏ sót bất kỳ OR nào. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu thứ hai:```
2
2 1 2
1 2
```Bước qua mảng. Khởi tạo`seen = {}`Và`prev = {}`. Số đầu tiên là`0b11`.`current = {0b11}`.`0b11`không ở trong`seen`, vì vậy hãy thêm vào`seen`. Hiện nay`seen = {0b11}`,`prev = {0b11}`. Số tiếp theo là`0b10`.`current = {0b10}`cộng thêm`0b10 | 0b11 = 0b11`.`0b11`đã ở trong rồi`seen`, vì vậy chúng tôi in "Có". Điều này xác nhận rằng các giá trị OR có thể lặp lại trên các chuỗi con riêng biệt. 

Đối với đầu vào mẫu đầu tiên:```
2 1 5
2 2 4
2 2 3
```OR lan truyền mà không có va chạm.`seen`sau mỗi bước trở thành`{0b10001}`, sau đó`{0b10001,0b1010}`, sau đó`{0b10001,0b1010,0b110}`. Không có sự trùng lặp nào xảy ra nên kết quả là "Không". Điều này thể hiện việc xử lý chính xác các OR không va chạm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng k * B) | Mỗi bit được xử lý một lần cho mỗi kết hợp OR. Tổng số k_i là ≤10^5; mỗi OR ảnh hưởng tối đa ~ 18 bit. | 
| Không gian | O(B) | Chúng tôi lưu trữ các bộ OR bị giới hạn bởi số bit trong phần tử lớn nhất, khoảng 18-19. | 

Thuật toán chạy thoải mái trong giới hạn 2 giây ngay cả ở những giới hạn lớn nhất vì tổng số thao tác tỷ lệ thuận với tổng của tất cả k_i nhân với số bit tối đa, khoảng 2 triệu thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("5\n3\n2 1 5\n2 2 4\n2 2 3\n2\n2 1 2\n1 2\n4\n3 1 2 4\n2 2 4\n4 1 2 5 6\n2 2 5\n5\n3 3 1 2\n3 2 5 3\n5 7 2 3 1 4\n5 1 2 6 3 5\n3 2 6 3\n2\n1 1\n1 2\n") == "No\nYes\nYes\nYes\nNo", "provided samples"

# Custom cases
assert run("1\n3\n1 1\n1 2\n1 3\n") == "No", "all single bit, distinct"
assert run("1\n4\n2 1 2\n2 2 3\n2 1 3\n1 1\n") == "Yes", "overlapping ORs"
assert run("1\n2\n1 1\n1 1\n") == "Yes", "duplicate identical numbers"
assert run("1\n1\n1 1\n") == "No", "single element, no subsequence pair"
assert run("1\n3\n2 1 2\n2 3 4\n2 1 2\n") == "Yes", "duplicate OR in non-consecutive elements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 số có bit phân biệt | Không | OR không va chạm | 
| OR chồng chéo | Có | Phát hiện chính xác các chuỗi con có cùng OR | 
| trùng lặp số giống nhau | Có | Xử lý chính xác các phần tử lặp lại |
