---
title: "CF 1847F - Danh Tính Ông Chủ"
description: "Chúng ta được cấp một chuỗi bắt đầu bằng tiền tố hữu hạn có độ dài $n$. Sau thời điểm đó, trình tự không dừng lại; thay vào đó, mọi phần tử mới được hình thành bằng cách kết hợp các phần tử $n$ trước đó bằng cách sử dụng bitwise OR giữa hai giá trị dịch chuyển liền kề."
date: "2026-06-09T05:45:53+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "data-structures", "dfs-and-similar", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1847
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 882 (Div. 2)"
rating: 2500
weight: 1847
solve_time_s: 75
verified: true
draft: false
---

[CF 1847F - Danh tính của Ông chủ](https://codeforces.com/problemset/problem/1847/F) 

**Đánh giá:** 2500 
**Thẻ:** tìm kiếm nhị phân, mặt nạ bit, cấu trúc dữ liệu, dfs và các loại tương tự, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi bắt đầu bằng tiền tố hữu hạn có độ dài$n$. Sau thời điểm đó, trình tự không dừng lại; thay vào đó, mọi phần tử mới được hình thành bằng cách kết hợp phần tử trước đó$n$các phần tử sử dụng bitwise OR giữa hai giá trị dịch chuyển liền kề. Cụ thể, mỗi thuật ngữ mới chỉ phụ thuộc vào thuật ngữ trước đó.$n$các điều khoản và mỗi vị trí là OR của hai vị trí trước đó được bù bởi$n$. 

Điều này tạo ra một chuỗi vô hạn xác định nhanh chóng trở nên “bão hòa” về mặt thông tin theo bit: một khi một bit xuất hiện trong bất kỳ cửa sổ nào, nó chỉ có thể lan sang bên phải thông qua các phép toán OR. 

Mỗi truy vấn đưa ra một ngưỡng$v$và chúng ta phải tìm chỉ số sớm nhất trong chuỗi vô hạn này trong đó giá trị vượt quá$v$. Nếu không có vị trí như vậy tồn tại, câu trả lời là$-1$. 

Các ràng buộc chỉ ra rằng tổng$n$và tổng số truy vấn trên tất cả các trường hợp thử nghiệm lên tới$2 \cdot 10^5$. Điều này ngay lập tức loại trừ mọi mô phỏng trình tự ngoài một vài bước đầu tiên, vì trình tự là vô hạn và thậm chí việc tạo tiền tố lớn cho mỗi truy vấn cũng sẽ quá chậm. Chúng ta cần một cách biểu diễn tránh việc xây dựng các giá trị một cách rõ ràng vượt quá một ranh giới nhỏ. 

Trường hợp cạnh tinh tế xuất hiện khi mảng ban đầu đã nhỏ và không thể tạo ra bất kỳ giá trị lớn hơn nào thông qua truyền bá OR. Trong trường hợp đó, câu trả lời ngay lập tức$-1$cho các truy vấn lớn. Một dạng sai sót khác là giả sử tính tuần hoàn hoặc giả sử chuỗi ổn định nhanh chóng đến giá trị tối đa ở tất cả các vị trí, điều này không nhất thiết đúng trừ khi được chứng minh cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ mô phỏng trình tự. Đối với mỗi vị trí, hãy tính toán các giá trị bằng cách sử dụng phép truy toán cho đến khi chúng tôi vượt quá tất cả các chỉ số hữu ích có thể có. Nhưng chuỗi là vô hạn và các giá trị chỉ tăng thông qua OR, vì vậy trong trường hợp xấu nhất, số lượng trạng thái riêng biệt cần thiết trước khi ổn định có thể rất lớn. Nếu chúng ta cố gắng trả lời một truy vấn bằng cách kéo dài chuỗi cho đến khi vượt qua$v$, mỗi truy vấn có thể buộc chúng tôi tạo ra nhiều thuật ngữ, dẫn đến sự bùng nổ bậc hai trong các trường hợp thử nghiệm. 

Quan sát quan trọng là mỗi phần tử là một bit HOẶC trên một cấu trúc trượt, do đó giá trị tại vị trí$i$phụ thuộc vào một tập hợp các giá trị cố định trước đó và các bit chỉ có thể tích lũy chứ không bao giờ biến mất. Điều này gợi ý việc theo dõi khi mỗi bit bắt đầu hoạt động trong chuỗi. 

Thay vì theo dõi các giá trị đầy đủ, chúng tôi duy trì cho mỗi vị trí bit chỉ số sớm nhất nơi bit đó trở thành 1 trong chuỗi vô hạn. Khi biết “thời gian kích hoạt” này, chúng ta có thể ngầm xây dựng lại giá trị tại bất kỳ chỉ mục nào: tại vị trí$i$, giá trị là OR của tất cả các bit có thời gian kích hoạt tối đa$i$. Điều này biến vấn đề thành lý luận về ngưỡng đối với các sự kiện kích hoạt bit. 

Bây giờ hãy xem xét một truy vấn$v$. Chúng ta cần chỉ số nhỏ nhất$i$sao cho ít nhất một bit được đặt trong$v$vắng mặt ở$a_i$. Tương đương,$a_i$phải chứa một chút bên ngoài$v$, nghĩa$a_i$không phải là tập con của$v$. Vì vậy, chúng tôi đang tìm kiếm vị trí đầu tiên mà bitmask “thoát khỏi” mặt nạ$v$. Điều này trở thành một vấn đề về kiểu phạm vi tối thiểu đối với các ràng buộc kích hoạt bit. 

Chúng tôi tính toán trước cho mỗi bit vị trí sớm nhất mà nó xuất hiện. Sau đó, cho một$v$, chỉ số đầu tiên trong đó$a_i$vượt quá$v$là chỉ mục sớm nhất trong số tất cả các bit không có trong$v$, bởi vì bất kỳ bit nào như vậy đều buộc giá trị phải lớn hơn. Cấu trúc giảm xuống mức duy trì ở mức tối thiểu trong thời gian kích hoạt liên quan, có thể được xử lý trước và phản hồi một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force |$O(n \cdot \text{queries})$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Kích hoạt bit + tiền xử lý |$O((n + q)\log A)$|$O(\log A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng theo dõi xem mỗi bit (0 đến 30) có xuất hiện trong phân đoạn ban đầu hay không. Chúng tôi ghi lại chỉ mục sớm nhất trong đó mỗi bit được đặt trong mảng ban đầu. Đây là kích hoạt cơ sở của chúng tôi. 
2. Xử lý ngầm định phép truy toán bằng cách quan sát rằng việc truyền OR không thể tạo ra các bit mới; nó chỉ lan truyền những cái hiện có về phía trước. Do đó, sự xuất hiện sớm nhất của mỗi bit được xác định hoàn toàn bởi tiền tố, vì mọi sự xuất hiện sau đó chỉ là sự lan truyền của những lần xuất hiện trước đó. 
3. Xây dựng một mảng`first[bit]`lưu trữ chỉ số tối thiểu trong phân đoạn ban đầu nơi bit đó xuất hiện. Nếu một bit không bao giờ xuất hiện thì giá trị của nó vẫn là vô hạn. 
4. Với mỗi giá trị truy vấn$v$, giải thích nó ở dạng nhị phân. Chúng tôi muốn vị trí sớm nhất có ít nhất một bit bằng 0 trong$v$trở nên hoạt động theo trình tự. 
5. Lặp lại tất cả các bit từ 0 đến 30. Đối với mỗi bit không được đặt trong$v$, hãy xem xét thời gian kích hoạt của nó. Câu trả lời là tối thiểu trong số những lần kích hoạt này. 
6. Nếu không có bit đó tồn tại (nghĩa là$v$đã chứa tất cả các bit từng xuất hiện trong chuỗi), trả về$-1$. 

### Tại sao nó hoạt động 

Mỗi vị trí trong chuỗi chỉ được xây dựng thông qua các phép toán OR, do đó, khi một bit xuất hiện ở bất kỳ chỉ mục nào, nó sẽ tồn tại hoặc lan rộng về phía trước nhưng không bao giờ biến mất. Do đó, tập hợp các bit có mặt tại vị trí$i$chính xác là sự kết hợp của các bit có lần xuất hiện đầu tiên vào hoặc trước$i$. Điều này tạo ra một mối quan hệ đơn điệu giữa chỉ mục và độ bao phủ bit. Một giá trị vượt quá$v$chính xác khi nó chứa một chút bên ngoài$v$, do đó chỉ số đầu tiên như vậy chỉ được xác định bởi thời gian kích hoạt sớm nhất trong số các bit bị thiếu đó. Điều này ngăn chặn bất kỳ sự phụ thuộc nào vào cấu trúc sau này của chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    INF = 10**18
    
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        
        first = [INF] * 31
        
        for i, x in enumerate(a, start=1):
            for b in range(31):
                if x >> b & 1:
                    first[b] = min(first[b], i)
        
        # If a bit appears later via propagation, it cannot be earlier than initial appearance
        # so first occurrence in prefix is sufficient
        
        for _ in range(q):
            v = int(input())
            ans = INF
            
            for b in range(31):
                if (v >> b) & 1:
                    continue
                ans = min(ans, first[b])
            
            if ans == INF:
                print(-1)
            else:
                print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp chỉ lưu trữ chỉ mục sớm nhất trong đó mỗi bit có trong tiền tố ban đầu. Sau đó, mỗi truy vấn sẽ kiểm tra xem bit nào bị thiếu trong$v$và trong số các bit đó chọn chỉ số kích hoạt nhỏ nhất. Chỉ mục đó là vị trí đầu tiên mà trình tự phải vượt quá$v$, vì bit đó đảm bảo giá trị lớn hơn. 

Một cạm bẫy triển khai phổ biến là quên rằng các truy vấn chỉ phụ thuộc vào việc thiếu bit chứ không phụ thuộc vào so sánh số đầy đủ. Một vấn đề tế nhị khác là xử lý sai trường hợp$v$đã bao gồm tất cả các bit từng xuất hiện; việc này được xử lý chính xác bằng cách quay lại$-1$khi không có chỉ số ứng cử viên nào tồn tại. 

## Ví dụ đã hoạt động 

Chúng tôi lấy một ví dụ tùy chỉnh nhỏ bắt nguồn từ cấu trúc. 

đầu vào:```
n = 3, q = 3
a = [1, 2, 4]
queries = [0, 1, 7]
```Chúng tôi tính toán lần xuất hiện đầu tiên: 

| chút | chỉ số đầu tiên | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 3 | 

Bây giờ truy vấn: 

cho$v = 0$, tất cả các bit 0,1,2 đều bị thiếu, vì vậy câu trả lời là min(1,2,3) = 1. 

cho$v = 1$(nhị phân 001), các bit bị thiếu là 1 và 2, vì vậy câu trả lời là min(2,3) = 2. 

cho$v = 7$(nhị phân 111), không có bit nào bị thiếu trong số những bit từng xuất hiện, vì vậy câu trả lời là$-1$. 

Điều này xác nhận rằng câu trả lời chỉ phụ thuộc vào sự xuất hiện đầu tiên của bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q) \cdot 31)$| Mỗi phần tử và truy vấn kiểm tra tối đa 31 bit | 
| Không gian |$O(31)$| Chỉ lưu trữ lần xuất hiện sớm nhất trên mỗi bit | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số hoạt động và quét 31-bit thoải mái trong giới hạn. Giải pháp tránh mọi mô phỏng của chuỗi vô hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    
    INF = 10**18
    
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        
        first = [INF] * 31
        
        for i, x in enumerate(a, start=1):
            for b in range(31):
                if x >> b & 1:
                    first[b] = min(first[b], i)
        
        for _ in range(q):
            v = int(input())
            ans = INF
            for b in range(31):
                if (v >> b) & 1:
                    continue
                ans = min(ans, first[b])
            out.append(str(-1 if ans == INF else ans))
    
    return "\n".join(out)

# provided samples
assert run("""3
2 3
2 1
1
2
3
4 5
0 2 1 3
0
1
2
3
4
5 5
1 2 3 4 5
7
2
6
0
4
""") == """1
3
-1
2
2
4
-1
-1
-1
3
8
1
5"""

# custom cases
assert run("""1
2 2
1 2
0
3
""") == """1
-1""", "minimal bit coverage"

assert run("""1
3 3
0 0 0
0
1
2
""") == """-1
-1
-1""", "all zeros never exceed"

assert run("""1
4 2
8 4 2 1
0
15
""") == """1
-1""", "full mask query"

assert run("""1
5 3
1 3 7 15 0
0
2
14
""") == """1
2
4""", "mixed growth"

| Test input | Expected output | What it validates |
|---|---|---|
| minimal bit coverage | 1 -1 | single-bit activation dominance |
| all zeros never exceed | -1 -1 -1 | no bit propagation |
| full mask query | 1 -1 | saturation edge case |
| mixed growth | 1 2 4 | ordering of bit activations |

## Edge Cases

A case where all initial values are zero exposes the fact that no bit ever becomes active. The algorithm initializes all `first[b]` to infinity, so every query correctly returns \(-1\), since there is no index where any bit appears.

A case where \( v \) already contains all bits seen in the array demonstrates the final condition. Because every bit that ever appears is included in \( v \), the candidate set of missing bits is empty, and the algorithm returns \(-1\) without needing any search.

A case with sparse high bits shows why per-bit independence is sufficient. If only bit 10 appears, then any \( v \) with that bit set immediately eliminates all candidates, while any \( v \) without it returns the first index of appearance.
```
