---
title: "CF 1203A - Vòng tròn sinh viên"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, một nhóm học sinh được sắp xếp theo một thứ tự vòng tròn cố định và chúng ta được hiển thị thứ tự đó dưới dạng một danh sách tuyến tính biểu thị sự di chuyển theo chiều kim đồng hồ quanh vòng tròn."
date: "2026-06-13T15:35:08+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1203
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 579 (Div. 3)"
rating: 1000
weight: 1203
solve_time_s: 420
verified: true
draft: false
---

[CF 1203A - Vòng tròn sinh viên](https://codeforces.com/problemset/problem/1203/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 7 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, một nhóm học sinh được sắp xếp theo một thứ tự vòng tròn cố định và chúng ta được hiển thị thứ tự đó dưới dạng một danh sách tuyến tính biểu thị sự di chuyển theo chiều kim đồng hồ quanh vòng tròn. 

Mỗi học sinh có một nhãn duy nhất từ ​​1 đến n và mảng đã cho mô tả nhãn nào xuất hiện ở mỗi vị trí khi chúng ta đi quanh vòng tròn theo chiều kim đồng hồ. 

Nhiệm vụ là xác định xem liệu sự sắp xếp vòng tròn này có thể được hiểu là một “vũ điệu liên tiếp hoàn hảo” theo cả hai hướng hay không. Trong một trường hợp hợp lệ, bắt đầu từ một học sinh nào đó, di chuyển theo chiều kim đồng hồ sẽ tạo ra dãy 1, 2, 3, ..., n theo thứ tự không ngắt quãng. Trong trường hợp hợp lệ khác, di chuyển theo chiều kim đồng hồ sẽ tạo ra một chuỗi đảo ngược n, n−1, ..., 1. Vì đường tròn không có điểm bắt đầu cố định nên chúng ta được phép xoay chuỗi nhưng không được phép sắp xếp lại nó. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem hoán vị vòng tròn đã cho là một phép quay của dãy tăng hay dãy giảm. 

Các ràng buộc nhỏ, với n lên tới 200 và q lên tới 200. Điều này có nghĩa là ngay cả một giải pháp kiểm tra từng vòng quay hoặc kiểm tra trực tiếp từng vị trí cũng đủ nhanh một cách dễ dàng. Quét tuyến tính cho mỗi trường hợp thử nghiệm là đủ. 

Một trường hợp cạnh tinh tế xuất hiện khi n bằng 1 hoặc 2. Với n = 1, mọi sự sắp xếp đều có giá trị tầm thường. Với n = 2, cả [1, 2] và [2, 1] đều hợp lệ, nhưng bất kỳ sự lặp lại nào như [1, 1] đều không thể xảy ra vì đầu vào được đảm bảo là một hoán vị, do đó tính hợp lệ chỉ phụ thuộc vào tính nhất quán kề cận. 

Một lỗi phổ biến là chỉ kiểm tra xem các khác biệt liền kề là +1 hay −1 mà không xem xét tính nhất quán bao quanh. Ví dụ: [2, 3, 4, 5, 1] ​​hợp lệ ngay cả khi 1 không liền kề với 2 theo thứ tự chỉ số; bọc tròn làm cho nó hợp lệ. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để nghĩ về điều này là thử mọi điểm bắt đầu có thể có trong vòng tròn. Đối với mỗi chỉ số bắt đầu i, chúng tôi mô phỏng việc đi bộ theo chiều kim đồng hồ và kiểm tra xem liệu chúng tôi có thể đọc từ 1 đến n theo thứ tự hay n đến 1 theo thứ tự. Điều này yêu cầu O(n) hoạt động trên mỗi điểm bắt đầu, đưa ra O(n^2) cho mỗi trường hợp thử nghiệm. 

Mặc dù điều này đã được chấp nhận với n 200, nhưng chúng ta có thể đơn giản hóa hơn nữa bằng cách sử dụng một quan sát chính: trong một cách sắp xếp hợp lệ, nếu chúng ta di chuyển theo chiều kim đồng hồ dọc theo đường tròn, thì hiệu giữa các phần tử liên tiếp phải luôn luôn bằng +1 (mod n trong không gian giá trị) hoặc nhất quán −1 (mod n trong không gian giá trị). Cấu trúc cứng nhắc: một khi chúng ta cố định hướng, toàn bộ hoán vị sẽ được xác định. 

Vì vậy, thay vì thử tất cả các phép quay, chúng tôi chỉ kiểm tra xem tất cả các sai phân liền kề có nhất quán với việc tăng lên một hay giảm đi một khi diễn giải theo chu kỳ hay không. 

Chúng ta có thể thực hiện điều này bằng cách kiểm tra từng vị trí xem p[i+1] là p[i]+1 (hoặc quấn từ n đến 1), hay p[i+1] là p[i]−1 (hoặc quấn từ 1 đến n). Nếu một trong hai điều kiện đúng cho tất cả các cạnh bao gồm cạnh cuối cùng đến đầu tiên thì câu trả lời là CÓ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra vòng quay Brute Force | O(n^2) | O(1) | Đã chấp nhận | 
| Kiểm tra tính nhất quán của hướng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc hoán vị cho truy vấn hiện tại, biểu thị thứ tự theo chiều kim đồng hồ xung quanh vòng tròn. Trình tự này là cố định và không thể sửa đổi. 
2. Giả sử chúng ta cố gắng xác nhận chuỗi tăng dần theo chiều kim đồng hồ. Chúng tôi xác minh xem mỗi cặp liên tiếp có tuân theo quy tắc giá trị tăng chính xác bằng 1 hay không, với trường hợp đặc biệt là n kết nối trở lại 1. 
3. Nếu kiểm tra tăng không thành công, hãy thử phiên bản giảm, trong đó mỗi bước phải giảm đúng 1, với trường hợp đặc biệt là 1 kết nối ngược lại n. 
4. Đối với cả hai lần kiểm tra, chúng ta phải coi chuỗi là vòng tròn, nghĩa là phần tử cuối cùng kết nối trở lại phần tử đầu tiên. Điều này đảm bảo chúng ta đang thử nghiệm một vòng lặp đầy đủ chứ không phải một đường đứt đoạn. 
5. Nếu một trong hai hướng hợp lệ cho mọi cặp liền kề, ghi CÓ. Nếu không thì xuất ra NO. 

### Tại sao nó hoạt động 

Một điệu nhảy vòng tròn hợp lệ đòi hỏi việc đi vòng quanh vòng tròn luôn tạo ra các nhãn liên tiếp theo một hướng nhất quán. Điều này thực thi một ràng buộc toàn cục: mỗi nút có chính xác một nút kế thừa hợp lệ theo hướng truyền tải. Bởi vì các nhãn là một hoán vị, nếu ngay cả một cạnh kề vi phạm quy tắc +1 hoặc −1 thì không thể xoay vòng tròn thành một chuỗi hoàn hảo. Ngược lại, nếu tất cả các vùng kề thỏa mãn một trong hai quy tắc bước nhất quán thì toàn bộ chu trình phải tạo thành một chu trình có hướng duy nhất bao gồm tất cả các số nguyên theo thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok_inc(p):
    n = len(p)
    for i in range(n):
        cur = p[i]
        nxt = p[(i + 1) % n]
        if cur == n:
            if nxt != 1:
                return False
        else:
            if nxt != cur + 1:
                return False
    return True

def ok_dec(p):
    n = len(p)
    for i in range(n):
        cur = p[i]
        nxt = p[(i + 1) % n]
        if cur == 1:
            if nxt != n:
                return False
        else:
            if nxt != cur - 1:
                return False
    return True

q = int(input())
for _ in range(q):
    n = int(input())
    p = list(map(int, input().split()))
    if ok_inc(p) or ok_dec(p):
        print("YES")
    else:
        print("NO")
```Mã tách hai cấu trúc hợp lệ có thể có thành hai kiểm tra rõ ràng. Mỗi hàm đi qua tất cả các cạnh của sắp xếp hình tròn và xác minh xem mối quan hệ kế tiếp có khớp với cấp số cộng nghiêm ngặt theo modulo n hay không. 

Việc sử dụng lập chỉ mục modulo`(i + 1) % n`là rất quan trọng vì phần tử cuối cùng phải kết nối trở lại phần tử đầu tiên, nếu không chúng ta sẽ chỉ xác thực một đường dẫn thay vì một chu trình. 

## Ví dụ đã hoạt động 

### Ví dụ 1: p = [1, 2, 3, 5, 4] 

Chúng tôi kiểm tra hướng tăng đầu tiên. 

| tôi | cur | nxt | Dự kiến ​​| Có hiệu lực cho đến nay | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 2 = 1+1 | CÓ | 
| 1 | 2 | 3 | 3 = 2+1 | CÓ | 
| 2 | 3 | 5 | 4 dự kiến ​​| KHÔNG | 

Việc kiểm tra tăng dần không thành công khi chuyển từ 3 sang 5. Sau đó chúng tôi kiểm tra thứ tự giảm dần. 

| tôi | cur | nxt | Dự kiến ​​| Có hiệu lực cho đến nay | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 0 gói dự kiến ​​| KHÔNG ngay lập tức | 

Cả hai hướng đều không hoạt động nên đầu ra là KHÔNG. 

Điều này chứng tỏ rằng một sự kề cận bị hỏng cũng đủ để làm mất hiệu lực toàn bộ chu trình. 

### Ví dụ 2: p = [3, 2, 1, 4] 

Tăng cường kiểm tra: 

| tôi | cur | nxt | Dự kiến ​​| hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 2 | 4 dự kiến ​​| KHÔNG | 

Kiểm tra giảm: 

| tôi | cur | nxt | Dự kiến ​​| hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 2 | 2 = 3−1 | CÓ | 
| 1 | 2 | 1 | 1 = 2−1 | CÓ | 
| 2 | 1 | 4 | 4 = 1−1 bọc | CÓ | 
| 3 | 4 | 3 | 3 = 4−1 | CÓ | 

Điều này xác nhận rằng các chuỗi hợp lệ không bắt buộc phải bắt đầu từ 1; bất kỳ vòng quay nào của một chu kỳ giảm hoàn hảo đều có thể chấp nhận được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | Mỗi lần kiểm tra sẽ quét mảng hình tròn một lần | 
| Không gian | O(1) | Chỉ sử dụng các biến phụ không đổi | 

Với q 200 và n 200, tổng công việc tối đa là 40.000 thao tác, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def ok_inc(p):
        n = len(p)
        for i in range(n):
            cur = p[i]
            nxt = p[(i + 1) % n]
            if cur == n:
                if nxt != 1:
                    return False
            else:
                if nxt != cur + 1:
                    return False
        return True

    def ok_dec(p):
        n = len(p)
        for i in range(n):
            cur = p[i]
            nxt = p[(i + 1) % n]
            if cur == 1:
                if nxt != n:
                    return False
            else:
                if nxt != cur - 1:
                    return False
        return True

    q = int(input())
    out = []
    for _ in range(q):
        n = int(input())
        p = list(map(int, input().split()))
        out.append("YES" if (ok_inc(p) or ok_dec(p)) else "NO")
    return "\n".join(out)

# provided samples
assert run("""5
4
1 2 3 4
3
1 3 2
5
1 2 3 5 4
1
1
5
3 2 1 5 4
""") == """YES
YES
NO
YES
YES"""

# custom cases
assert run("""1
2
1 2
""") == "YES", "minimum increasing"

assert run("""1
2
2 1
""") == "YES", "minimum decreasing"

assert run("""1
4
1 3 2 4
""") == "NO", "broken cycle"

assert run("""1
3
2 3 1
""") == "YES", "rotation of increasing cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 tăng | CÓ | chu kỳ tăng hợp lệ nhỏ nhất | 
| n=2 giảm | CÓ | chu kỳ giảm hợp lệ nhỏ nhất | 
| 1 3 2 4 | KHÔNG | phát hiện vi phạm cục bộ | 
| 2 3 1 | CÓ | xoay đúng | 

## Vỏ cạnh 

Với n = 1, cả hai lần kiểm tra đều thành công một cách dễ dàng vì không có chuyển đổi nào vi phạm tính nhất quán, do đó, một học sinh tạo thành một chu trình hợp lệ theo định nghĩa. 

Đối với các chu kỳ nhỏ như n = 2, cả hai hướng đều thu gọn vào cùng một kiểm tra kề cận và thuật toán chấp nhận chính xác cả [1, 2] và [2, 1] vì mỗi hướng đều thỏa mãn một trong các quy tắc kế thừa nghiêm ngặt. 

Đối với các phép quay chẳng hạn như [3, 4, 5, 1, 2], việc kiểm tra tăng dần thành công ngay cả khi chuỗi không bắt đầu ở 1. Điều kiện modulo ở cuối mảng đảm bảo tính chính xác bao quanh, xác nhận rằng cấu trúc thực sự là hình tròn chứ không phải tuyến tính.
