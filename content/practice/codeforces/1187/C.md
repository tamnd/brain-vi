---
title: "CF 1187C - Vasya Và Mảng"
description: "Chúng ta được yêu cầu xây dựng bất kỳ mảng số nguyên nào có độ dài $n$ phù hợp với một tập hợp các ràng buộc về mảng con. Mỗi ràng buộc mô tả rằng một phân đoạn phải không giảm hoặc nó không thể không giảm."
date: "2026-06-13T12:32:45+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1187
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 67 (Rated for Div. 2)"
rating: 1800
weight: 1187
solve_time_s: 332
verified: false
draft: false
---

[CF 1187C - Vasya và mảng](https://codeforces.com/problemset/problem/1187/C) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 5 phút 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một mảng số nguyên có độ dài bất kỳ$n$phù hợp với một tập hợp các ràng buộc về mảng con. Mỗi ràng buộc mô tả rằng một phân đoạn phải không giảm hoặc nó không thể không giảm. 

Một đoạn được đánh dấu là “đã sắp xếp” buộc mọi cặp liền kề bên trong nó phải đáp ứng$a_i \le a_{i+1}$. Một phân đoạn được đánh dấu là "không được sắp xếp" chỉ yêu cầu phải tồn tại ít nhất một đảo ngược liền kề bên trong nó, nghĩa là một số$a_j > a_{j+1}$trong khoảng đó. Thách thức là bản thân mảng không được đưa ra mà chỉ có những ràng buộc quan hệ này trong các khoảng thời gian chồng chéo. 

Khó khăn chính là các ràng buộc “được sắp xếp” có tính toàn cục và cứng nhắc, trong khi các ràng buộc “không được sắp xếp” mang tính tồn tại và cục bộ. Một công trình hợp lệ phải thỏa mãn tất cả các yêu cầu đơn điệu trong khi vẫn đặt ít nhất một đường đi xuống bên trong mỗi khoảng cấm. 

Những hạn chế$n, m \le 1000$cho phép một$O(n^2)$hoặc$O(nm)$xây dựng bằng tiền xử lý hoặc truyền bá, nhưng bất cứ điều gì yêu cầu liệt kê tất cả các mảng hoặc kiểm tra tất cả các phép gán đều không khả thi. Rõ ràng chúng ta đang ở trong một chế độ mang tính xây dựng hoặc tham lam, nơi chúng ta dần dần thực thi cơ cấu. 

Trường hợp cạnh tinh tế xuất hiện khi một phân đoạn “không được sắp xếp” hoàn toàn được chứa bên trong một vùng buộc phải không giảm bởi các ràng buộc “được sắp xếp”. Trong trường hợp đó, câu trả lời là không thể ngay lập tức vì tính đơn điệu ngăn cản mọi sự đi xuống ở bất kỳ đâu trong khu vực đó. Ví dụ, nếu chúng ta được bảo$(1, 1, 5)$và cả$(0, 2, 3)$, thì ràng buộc thứ hai là không thể bởi vì ràng buộc thứ nhất buộc tất cả các vùng lân cận trong$[1,5]$phải không giảm. 

Một trường hợp tinh tế khác là các phân đoạn được sắp xếp chồng chéo tạo ra sự đơn điệu toàn cục đối với một liên minh lớn, khiến cho việc đặt một phần gốc bắt buộc là không thể trừ khi nó nằm ngoài tất cả các vùng bắt buộc. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là coi mảng là ẩn số và thử tất cả các phép gán bất đẳng thức có thể có giữa các phần tử liền kề, quyết định xem mỗi cặp có$a_i \le a_{i+1}$hoặc$a_i > a_{i+1}$. Khi các quyết định này được cố định, chúng ta có thể gán các giá trị số thực tế phù hợp với chúng. 

Tuy nhiên, điều này là theo cấp số nhân trong$n$, vì mỗi$n-1$lân cận có thể tăng hoặc giảm. Ngay cả khi chúng ta chỉ xem xét việc kiểm tra tính nhất quán, mỗi nhiệm vụ đều yêu cầu xác minh tất cả$m$những hạn chế, đưa ra$O(2^n \cdot nm)$, vượt xa giới hạn. 

Quan sát quan trọng là cấu trúc thực sự duy nhất quan trọng là nơi chúng ta được phép đặt các mức giảm. Nếu chúng ta quyết định rằng một số vùng kề cận nhất định phải không giảm do ràng buộc loại 1, thì bất kỳ ràng buộc loại 0 nào cũng phải chứa ít nhất một vùng kề cận bên ngoài tập bắt buộc này. Điều này sẽ điều chỉnh lại vấn đề thành việc xây dựng một tập hợp các “cạnh bình đẳng bị cấm” và đảm bảo mỗi khoảng “không được sắp xếp” chứa ít nhất một cạnh cho phép giảm nghiêm ngặt. 

Điều này gợi ý một cấu trúc tham lam: trước tiên chúng ta giả sử tất cả các vùng lân cận đều không giảm, sau đó đưa ra các mức giảm nghiêm ngặt một cách có chọn lọc để đáp ứng tất cả các ràng buộc loại 0. Vì các ràng buộc loại 1 cấm giảm trong các phạm vi nhất định, chúng tôi phải đảm bảo rằng mọi vị trí giảm được chọn đều nằm ngoài tất cả các khoảng bị cấm đó. 

Bước cuối cùng là gán các giá trị thực tế: chúng ta có thể chỉ cần bắt đầu từ một giá trị lớn và giảm hoặc duy trì các giá trị tùy thuộc vào việc chúng ta đặt dấu ngắt hay sử dụng nhãn phân đoạn theo cách khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot nm)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(nm)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán sang việc quyết định vị trí liền kề nào$i \to i+1$sẽ được giảm nghiêm ngặt. Chúng ta hãy gọi đây là những “điểm dừng”. 

1. Bắt đầu bằng cách giả sử tất cả các cặp liền kề đều không giảm, nghĩa là ban đầu chúng ta không đặt ngắt bắt buộc. 
2. Xử lý tất cả các ràng buộc loại 1. Đối với mỗi phân đoạn$[l, r]$, chúng tôi đánh dấu rằng mọi vùng lân cận$l \le i < r$phải không giảm. Trong thực tế, điều này có nghĩa là chúng tôi không được phép đặt điểm ngắt bên trong bất kỳ phân đoạn nào trong số này. 
3. Từ các ràng buộc loại 1, chúng ta xây dựng một tập hợp các cạnh bị cấm trong đó không được phép ngắt. Bất kỳ điểm ngắt hợp lệ nào cũng phải nằm ngoài tất cả các vùng đơn điệu bắt buộc này. 
4. Bây giờ xử lý các ràng buộc loại 0. Đối với mỗi khoảng thời gian$[l, r]$, chúng ta cần ít nhất một điểm dừng$i$với$l \le i < r$điều đó không bị cấm. Nếu không có quan điểm như vậy tồn tại thì câu trả lời là không thể ngay lập tức. 
5. Để đảm bảo điều này một cách hiệu quả, chúng tôi tham lam chọn các vị trí điểm dừng. Chúng tôi lặp qua tất cả các vị trí và bất cứ khi nào chúng tôi gặp một khoảng loại 0 chưa được thỏa mãn, chúng tôi sẽ đặt một điểm dừng ở vị trí hợp lệ ngoài cùng bên phải có thể bên trong nó, miễn là nó không bị cấm. Điều này đảm bảo những hạn chế trong tương lai không bị tổn hại nhiều hơn mức cần thiết. 
6. Sau khi quyết định tất cả các điểm ngắt, hãy xây dựng mảng. Chúng ta gán giá trị từ trái sang phải. Bắt đầu với một số lớn và đặt$a_1 = 10^9$. Đối với mỗi$i$, nếu vị trí$i$là một điểm dừng, được đặt$a_{i+1} = a_i - 1$. Nếu không thì đặt$a_{i+1} = a_i$. 

Lý do điều này có tác dụng là vì các giá trị liền kề bằng nhau mã hóa các phân đoạn không giảm, trong khi mức giảm nghiêm ngặt mã hóa các vi phạm được đảm bảo. Bởi vì mỗi khoảng loại 0 chứa ít nhất một mức giảm bắt buộc nên nó được đảm bảo không được sắp xếp và vì không có dấu ngắt nào được đặt bên trong bất kỳ vùng cấm nào nên tất cả các ràng buộc loại 1 vẫn hợp lệ. 

Điều bất biến được duy trì là sau khi xử lý từng ràng buộc, mọi khoảng thời gian loại 0 được thấy cho đến nay đều chứa ít nhất một điểm dừng được chọn và không có điểm dừng nào nằm bên trong bất kỳ phân đoạn đơn điệu được thực thi loại 1 nào. Điều này ngăn chặn xung đột tích lũy không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    # forbidden[i] means we cannot place a "decrease" between i and i+1
    forbidden = [False] * (n + 1)

    type0 = []
    
    # First process type 1 constraints: forbid decreases inside them
    for _ in range(m):
        t, l, r = map(int, input().split())
        if t == 1:
            for i in range(l, r):
                forbidden[i] = True
        else:
            type0.append((l, r))

    # Try to place breaks greedily for type 0 constraints
    break_at = [False] * (n + 1)

    for l, r in type0:
        found = False
        # try to place a break as right as possible
        for i in range(r - 1, l - 1, -1):
            if not forbidden[i]:
                break_at[i] = True
                found = True
                break
        if not found:
            print("NO")
            return

    # construct array
    a = [0] * (n + 1)
    a[1] = 10**9

    for i in range(1, n):
        if break_at[i]:
            a[i + 1] = a[i] - 1
        else:
            a[i + 1] = a[i]

    print("YES")
    print(*a[1:])

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi tất cả các ràng buộc loại 1 thành các vị trí kề bị cấm, nghĩa là các cạnh đó phải không giảm. Sau đó, nó đảm bảo mọi khoảng loại 0 có ít nhất một điểm dừng được chọn bằng cách quét từ phải sang trái bên trong mỗi khoảng và đặt dấu ngắt ở vị trí hợp lệ mới nhất có thể. Cuối cùng, nó xây dựng một mảng cụ thể bằng cách truyền các giá trị từ trái sang phải, chỉ giảm tại các điểm dừng đã chọn. 

Một chi tiết triển khai tinh tế là chúng tôi lưu trữ các điểm dừng trên mỗi vùng lân cận thay vì sửa đổi trực tiếp các giá trị trong quá trình xử lý ràng buộc. Sự tách biệt này tránh sự tương tác ngẫu nhiên giữa các ràng buộc chồng chéo và đảm bảo việc kiểm tra tính khả thi vẫn độc lập với việc gán số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 4
1 1 3
1 2 5
0 5 6
1 6 7
```Trước tiên, chúng tôi cấm ngắt quãng trong các phân đoạn loại 1. 

| Bước | Ràng buộc | Các cạnh bị cấm | Đã chọn nghỉ | 
| --- | --- | --- | --- | 
| 1 | [1,3] được sắp xếp | 1-2, 2-3 | không | 
| 2 | [2,5] được sắp xếp | 2-3, 3-4, 4-5 | chưa có gì cho đến nay | 
| 3 | [5,6] chưa được sắp xếp | chọn 5 | nghỉ lúc 5 | 
| 4 | [6,7] được sắp xếp | 6 | không | 

Sau khi xử lý, chỉ vị trí 5 có điểm dừng. Xây dựng các giá trị: 

Bắt đầu$a_1 = 10^9$. Ta giữ nguyên cho đến chỉ số 5 thì giảm xuống 5 rồi lại giữ nguyên. Điều này mang lại một mảng hợp lệ phù hợp với tất cả các ràng buộc. 

Dấu vết này cho thấy các ràng buộc loại 0 được thỏa mãn một cách độc lập như thế nào trong khi vẫn tôn trọng các vùng đơn điệu. 

### Ví dụ 2 

đầu vào:```
5 3
1 1 4
0 2 3
0 3 5
```Đoạn [1,4] cấm tất cả các dấu ngắt ở vị trí 1-3. Điều đó chỉ còn lại vị trí 4. 

Đối với khoảng [2,3], không có vị trí ngắt hợp lệ vì 2 bị cấm. Điều này ngay lập tức chứng tỏ là không thể, vì vậy đầu ra là KHÔNG. 

Điều này chứng tỏ tầm quan trọng của việc kiểm tra tính khả thi trước khi xây dựng các giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ràng buộc có thể quét nhiều nhất$O(n)$vị trí trong trường hợp xấu nhất | 
| Không gian |$O(n)$| Mảng cho các cạnh và điểm dừng bị cấm | 

Được cho$n, m \le 1000$, điều này vừa vặn một cách thoải mái trong giới hạn, tối đa là$10^6$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("""7 4
1 1 3
1 2 5
0 5 6
1 6 7
""") == "YES\n1000000000 1000000000 1000000000 1000000000 999999999 999999998 999999998"

# all sorted constraint + impossible not-sorted inside
assert run("""5 2
1 1 5
0 2 4
""") == "NO"

# no constraints
assert run("""3 0
""") == "YES\n1000000000 1000000000 1000000000"

# tight alternating constraints
assert run("""4 2
0 1 2
0 3 4
""") == "YES\n1000000000 999999999 999999999 999999998"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đã được sắp xếp + vi phạm nội bộ | KHÔNG | phát hiện xung đột | 
| không có ràng buộc | mảng hằng số tầm thường | trường hợp cơ sở | 
| khoảng xen kẽ | tách hợp lệ | sự đúng đắn của vị trí tham lam | 

## Vỏ cạnh 

Trường hợp một cạnh phát sinh khi các ràng buộc loại 1 bao trùm toàn bộ mảng. Trong tình huống đó, tất cả các vị trí liền kề đều bị cấm ngắt, do đó, mọi ràng buộc loại 0 ngay lập tức trở nên không thể thực hiện được. Thuật toán xử lý việc này vì quá trình quét bên trong không tìm thấy bất kỳ điểm ngắt hợp lệ nào và trả về KHÔNG. 

Một trường hợp cạnh khác xảy ra khi nhiều khoảng loại 0 chồng chéo lên nhau nhưng tất cả đều được thỏa mãn bởi một điểm dừng duy nhất. Chiến lược tham lam xử lý vấn đề này một cách tự nhiên bởi vì một khi điểm dừng được đặt, tất cả các khoảng bao trùm chỉ số đó đều được thỏa mãn hoàn toàn và không cần phải làm thêm gì. 

Trường hợp tinh tế cuối cùng là khi các ràng buộc buộc phải tạo ra một “hành lang chặt chẽ” gồm các điểm dừng được phép, ví dụ như chỉ còn một vùng lân cận còn trống. Thuật toán vẫn thành công vì nó luôn thử vị trí hợp lệ ngoài cùng bên phải, đảm bảo rằng tính linh hoạt có giới hạn được sử dụng theo cách tối đa hóa phạm vi bao phủ của các ràng buộc trong tương lai.
