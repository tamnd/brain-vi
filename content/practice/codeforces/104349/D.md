---
title: "CF 104349D - Một vấn đề hoán vị khác"
description: "Chúng ta được cho hai hoán vị của cùng một tập hợp số từ 1 đến n. Mỗi người chơi sở hữu một mảng và trong một nước đi, người chơi được phép xóa bất kỳ phần tử nào khỏi mảng của chính họ."
date: "2026-07-01T18:15:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104349
codeforces_index: "D"
codeforces_contest_name: "TheForces Round #13 (Boombastic-Forces)"
rating: 0
weight: 104349
solve_time_s: 82
verified: false
draft: false
---

[CF 104349D - Một vấn đề hoán vị khác](https://codeforces.com/problemset/problem/104349/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai hoán vị của cùng một tập hợp số từ 1 đến n. Mỗi người chơi sở hữu một mảng và trong một nước đi, người chơi được phép xóa bất kỳ phần tử nào khỏi mảng của chính họ. Sau mỗi lần xóa, cả hai mảng đều co lại một cách hiệu quả và thứ tự tương đối của các phần tử còn lại được giữ nguyên. 

Trò chơi kết thúc khi cả hai mảng trở nên giống hệt nhau thành các chuỗi, nghĩa là chúng có cùng độ dài và khớp nhau ở mọi vị trí. Cả hai người chơi đều hành động tối ưu và nhiệm vụ là xác định tổng số lần di chuyển xóa tối thiểu cần thiết trên cả hai mảng để đạt được trạng thái giống hệt nhau này. 

Một cách hữu ích để diễn giải quá trình này là chúng ta đang cố gắng tìm một dãy con chung mà cả hai mảng có thể được rút gọn lại, vì việc xóa chỉ loại bỏ các phần tử trong khi vẫn giữ nguyên thứ tự. Do đó, mảng giống hệt cuối cùng phải là một chuỗi xuất hiện dưới dạng dãy con trong cả hai hoán vị. Trò chơi kết thúc khi cả hai bên đã xóa hết mọi thứ ngoại trừ dãy con chung đó. 

Do đó, câu trả lời được điều khiển bởi chuỗi dài nhất mà chúng ta có thể giữ nguyên trong cả hai mảng trong khi vẫn duy trì tính nhất quán về vị trí. 

Các ràng buộc rất mạnh: n có thể lên tới 100000 và được tính tổng trên các trường hợp thử nghiệm lên tới 100000. Điều này ngay lập tức loại trừ mọi cách tiếp cận bình phương bậc hai hoặc thậm chí n log cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào về cơ bản phải là tuyến tính hoặc tuyến tính. 

Một ý tưởng ngây thơ nhưng hấp dẫn là mô phỏng việc xóa hoặc thử tất cả các chuỗi cuối cùng có thể xảy ra. Điều đó không thành công vì số lượng các chuỗi con là theo cấp số nhân. Một nỗ lực không chính xác phổ biến khác là khớp các vị trí một cách tham lam mà không xem xét tính nhất quán toàn cầu, điều này sẽ bị phá vỡ khi các ràng buộc thứ tự tương đối xung đột giữa hai hoán vị. 

Ví dụ: nếu A = [1, 2, 3, 4] và B = [2, 1, 4, 3], người ta có thể tham lam cố gắng khớp các vị trí bằng nhau hoặc khớp sớm, nhưng bất kỳ quyết định cục bộ nào đều bỏ qua rằng chỉ một chuỗi con nhất quán toàn cầu mới có thể tồn tại khi bị xóa trong cả hai mảng. 

## Phương pháp tiếp cận 

Sự thay đổi quan điểm quan trọng là ngừng suy nghĩ về việc xóa bỏ và thay vào đó hãy nghĩ về những gì chúng ta có thể bảo tồn đồng thời. 

Vì cả hai mảng đều là hoán vị nên mỗi giá trị xuất hiện chính xác một lần trong mỗi mảng. Điều này cho phép chúng tôi mã hóa một hoán vị dưới dạng bản đồ vị trí so với hoán vị kia. Cụ thể, nếu cố định mảng A, chúng ta có thể ánh xạ từng giá trị x tới chỉ mục của nó trong A. Sau đó, mảng B trở thành một chuỗi các chỉ số tùy theo vị trí các giá trị của nó xuất hiện trong A. 

Điều này biến vấn đề thành một cấu trúc cổ điển: bây giờ chúng ta muốn dãy con tăng dài nhất của mảng được biến đổi đó. Bất kỳ dãy con tăng dần nào đều tương ứng với các giá trị xuất hiện theo cùng một thứ tự tương đối trong cả hai hoán vị, đó chính xác là những gì cần thiết để một dãy con chung có thể tồn tại khi bị xóa. 

Khi chúng tôi tính toán độ dài LIS, chẳng hạn như L, đại diện cho số phần tử tối đa có thể tồn tại trong cả hai mảng theo thứ tự giống hệt nhau. Vì cả hai mảng phải có kết quả chính xác bằng chuỗi được bảo toàn đó nên mỗi mảng phải xóa n − L phần tử. Do đó, tổng số lần xóa trên cả hai người chơi là 2(n − L). 

Cách tiếp cận bạo lực sẽ thử tất cả các chuỗi con của A và kiểm tra xem nó có xuất hiện trong B theo cùng thứ tự theo cấp số nhân hay không. Việc giảm LIS nén toàn bộ điều kiện tương thích thành một vấn đề tối ưu hóa trình tự đơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các chuỗi tiếp theo) | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu (LIS thông qua ánh xạ vị trí) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành từng trường hợp kiểm thử.

1. Xây dựng một mảng vị trí pos sao cho pos[x] là chỉ số của giá trị x trong hoán vị A. Điều này cho phép chúng ta dịch các giá trị sang thứ tự tương đối của chúng trong A. 
2. Chuyển đổi hoán vị B thành mảng T trong đó T[i] = pos[B[i]]. Bây giờ T đại diện cho B được biểu thị trong hệ tọa độ của A. 
3. Tính độ dài của dãy con tăng dài nhất của T bằng cách sử dụng kỹ thuật sắp xếp kiên nhẫn tiêu chuẩn với mảng đuôi. Mỗi phần tử được chèn bằng cách sử dụng tìm kiếm nhị phân. 
4. Gọi L là độ dài LIS. Đáp án cuối cùng là 2 * (n − L). 

Lý do bước 3 hợp lệ là vì bất kỳ dãy con tăng nào trong T đều tương ứng với các chỉ số trong A đang tăng, nghĩa là các giá trị tương ứng xuất hiện theo cùng thứ tự trong cả A và B. 

### Tại sao nó hoạt động 

LIS trên chuỗi được biến đổi sẽ nắm bắt chính xác tập hợp phần tử lớn nhất có thể được giữ lại mà không vi phạm trật tự trong cả hai hoán vị. Bởi vì mỗi số là duy nhất trong cả hai mảng, nên việc giữ nguyên thứ tự trong A tương đương với việc yêu cầu tăng chỉ số trong A và thứ tự khớp trong B được đảm bảo bằng cách xây dựng T. Mỗi lần xóa sẽ làm giảm các mảng một cách đối xứng cho đến khi chỉ còn lại cấu trúc nhất quán tối đa này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left

def lis_length(arr):
    tails = []
    for x in arr:
        i = bisect_left(tails, x)
        if i == len(tails):
            tails.append(x)
        else:
            tails[i] = x
    return len(tails)

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        A = list(map(int, input().split()))
        B = list(map(int, input().split()))
        
        pos = [0] * (n + 1)
        for i, v in enumerate(A):
            pos[v] = i
        
        T = [pos[v] for v in B]
        L = lis_length(T)
        out.append(str(2 * (n - L)))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng bản đồ vị trí cho A, điều này rất cần thiết để chuyển đổi hoán vị thứ hai thành một chuỗi số có thể so sánh được. Việc chuyển đổi thành T là bước rút gọn khóa và nó phải được thực hiện cẩn thận vì bất kỳ sai sót nào ở đây sẽ phá vỡ logic thứ tự. 

Hàm LIS sử dụng mảng đuôi tham lam với tìm kiếm nhị phân. Bất biến là tails[i] lưu trữ giá trị kết thúc nhỏ nhất có thể có của dãy con tăng dần có độ dài i+1. Điều này đảm bảo khả năng mở rộng tối ưu. 

Cuối cùng, phép tính trả lời phản ánh trực tiếp rằng cả hai mảng phải xóa tất cả các phần tử không nằm trong dãy con chung đã chọn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

A = [1, 2, 3, 4], B = [2, 1, 4, 3] 

Bản đồ vị trí từ A: 

1→0, 2→1, 3→2, 4→3 

Biến đổi B: 

T = [1, 0, 3, 2] 

Tính toán LIS: 

| Bước | x | đuôi | 
| --- | --- | --- | 
| 1 | 1 | [1] | 
| 2 | 0 | [0] | 
| 3 | 3 | [0, 3] | 
| 4 | 2 | [0, 2] | 

L = 2 

Đáp án = 2 * (4 − 2) = 4 

Điều này cho thấy chỉ có hai phần tử có thể được bảo toàn theo thứ tự nhất quán trên cả hai hoán vị. 

### Ví dụ 2 

đầu vào: 

A = [4, 2, 3, 1], B = [4, 3, 2, 1] 

Bản đồ vị trí từ A: 

4→0, 2→1, 3→2, 1→3 

Biến đổi B: 

T = [0, 2, 1, 3] 

LIS: 

| Bước | x | đuôi | 
| --- | --- | --- | 
| 1 | 0 | [0] | 
| 2 | 2 | [0, 2] | 
| 3 | 1 | [0, 1] | 
| 4 | 3 | [0, 1, 3] | 

L = 3 

Đáp án = 2 * (4 − 3) = 2 

Điều này chứng tỏ rằng ngay cả khi cả hai hoán vị được trộn lẫn nhiều, một chuỗi con nhất quán dài vẫn có thể tồn tại nếu cấu trúc tương đối căn chỉnh một phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tính toán LIS sử dụng tìm kiếm nhị phân cho từng phần tử | 
| Không gian | O(n) | mảng vị trí và chuỗi chuyển đổi | 

Tổng n trên các trường hợp thử nghiệm tối đa là 100000, do đó giải pháp phù hợp thoải mái trong giới hạn thời gian. Hệ số logarit đủ nhỏ để xử lý hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from bisect import bisect_left

    def lis_length(arr):
        tails = []
        for x in arr:
            i = bisect_left(tails, x)
            if i == len(tails):
                tails.append(x)
            else:
                tails[i] = x
        return len(tails)

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            A = list(map(int, input().split()))
            B = list(map(int, input().split()))
            pos = [0] * (n + 1)
            for i, v in enumerate(A):
                pos[v] = i
            T = [pos[v] for v in B]
            L = lis_length(T)
            out.append(str(2 * (n - L)))
        return "\n".join(out)

    return solve()

# provided sample
assert run("""3
4
1 2 3 4
2 1 4 3
4
1 2 3 4
1 2 4 3
5
4 2 3 5 1
4 5 1 3 2
""") == """4
2
4"""

# custom: already identical
assert run("""1
3
1 2 3
1 2 3
""") == "0"

# custom: reversed
assert run("""1
4
1 2 3 4
4 3 2 1
""") == "6"

# custom: alternating structure
assert run("""1
6
1 3 5 2 4 6
1 2 3 4 5 6
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoán vị giống hệt nhau | 0 | không cần xóa | 
| hoán vị ngược | 6 | LIS tối thiểu = 1 | 
| trường hợp xen kẽ hỗn hợp | 4 | nhất quán đặt hàng một phần | 

## Vỏ cạnh 

Một cặp hoán vị hoàn toàn giống hệt nhau tạo ra một chuỗi biến đổi tăng nghiêm ngặt, do đó LIS bằng n và kết quả trở thành 0. Thuật toán xử lý việc này một cách tự nhiên vì mọi phần tử đều mở rộng mảng tails mà không cần thay thế. 

Một hoán vị đảo ngược hoàn toàn tạo ra một chuỗi biến đổi giảm nghiêm ngặt, do đó LIS luôn bằng 1. Mã thay thế chính xác các mục nhập đuôi nhiều lần và kết thúc bằng độ dài 1, mang lại 2(n − 1). 

N trường hợp nhỏ như n = 1 hoặc n = 2 được xử lý mà không cần logic đặc biệt vì tính toán LIS và ánh xạ vị trí đều xuống cấp một cách an toàn: với n = 1, T có một phần tử và LIS là 1, không cho phép xóa như mong đợi.
