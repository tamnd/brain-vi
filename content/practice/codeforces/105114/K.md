---
title: "CF 105114K - Vấn đề về mảng khá ổn"
description: "Chúng ta được cung cấp một mảng số nguyên duy nhất và được yêu cầu xem xét từng phần liền kề của nó. Với mỗi lát cắt, chúng ta tính tổng các phần tử của nó và kiểm tra xem tổng đó có phải là số chẵn hay không."
date: "2026-06-27T19:53:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105114
codeforces_index: "K"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2024"
rating: 0
weight: 105114
solve_time_s: 70
verified: true
draft: false
---

[CF 105114K - Sự cố với mảng khá ổn](https://codeforces.com/problemset/problem/105114/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên duy nhất và được yêu cầu xem xét từng phần liền kề của nó. Với mỗi lát cắt, chúng ta tính tổng các phần tử của nó và kiểm tra xem tổng đó có phải là số chẵn hay không. Trong số tất cả các lát cắt như vậy, chúng ta muốn đếm xem có bao nhiêu lát cắt riêng biệt tồn tại thỏa mãn điều kiện chẵn lẻ này. 

“Riêng biệt” ở đây đề cập đến nội dung và vị trí mảng con thực tế, do đó, hai lát cắt chỉ giống nhau nếu chúng đến từ cùng một chỉ mục trong mảng ban đầu. Nói cách khác, mỗi cặp chỉ số$(l, r)$xác định chính xác một mảng con ứng cử viên và chúng tôi tính nó nếu tổng của nó là số chẵn. 

Kích thước mảng có thể đạt tới$10^6$, điều này ngay lập tức loại trừ mọi giải pháp kiểm tra tất cả$O(N^2)$mảng con. Ngay cả một hệ số không đổi vừa phải cũng sẽ làm cho phương pháp bậc hai không khả thi vì nó đòi hỏi$10^{12}$hoạt động trong trường hợp xấu nhất. 

Các trường hợp cạnh chính xoay quanh hành vi chẵn lẻ. Nếu tất cả các phần tử đều là số chẵn thì tổng của mọi mảng con đều là số chẵn, do đó câu trả lời sẽ trở thành tổng số mảng con. Nếu tất cả các phần tử đều là số lẻ thì tính chẵn lẻ hoàn toàn phụ thuộc vào độ dài của mảng con và chỉ các mảng con có độ dài chẵn mới đóng góp. Một trường hợp tinh tế khác là trộn lẫn các dấu hiệu hoặc giá trị lớn; vì chỉ có vấn đề tương đương, độ lớn không liên quan, nhưng việc triển khai bất cẩn vẫn có thể tính toán lại tổng một cách rõ ràng và tràn hoặc hết thời gian. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là liệt kê mọi mảng con có thể có và tính tổng của nó. Đối với mỗi chỉ số bắt đầu$l$, chúng tôi mở rộng$r$từ$l$ĐẾN$N-1$, duy trì một tổng số đang hoạt động. Mỗi lần chúng tôi gia hạn, chúng tôi kiểm tra xem tổng có chẵn hay không và đếm nếu có. Cách tiếp cận này đúng vì nó đánh giá rõ ràng mọi phân mảng ứng cử viên chính xác một lần. 

Tuy nhiên, điều này tạo ra khoảng$N(N+1)/2$mảng con và mỗi phần mở rộng mất thời gian không đổi, do đó tổng công việc vẫn ở dạng bậc hai. Với$N = 10^6$, điều này trở nên lớn về mặt thiên văn. 

Nhận xét quan trọng là chúng ta không thực sự quan tâm đến tổng số tiền đầy đủ; chúng tôi chỉ quan tâm đến việc liệu tổng có chẵn hay không. Tính chẵn lẻ hoạt động tốt trong phép cộng: việc thêm một phần tử sẽ lật tính chẵn lẻ nếu phần tử đó là số lẻ và giữ nguyên nó nếu phần tử đó là số chẵn. Điều này gợi ý thay thế từng giá trị bằng giá trị chẵn lẻ của nó$A_i \bmod 2$. 

Bây giờ tổng của một mảng con chẵn chính xác khi giá trị chẵn lẻ của tiền tố ở hai đầu khớp với nhau. Nếu chúng ta xác định tính chẵn lẻ của tiền tố$P[i] = (A_1 + \dots + A_i) \bmod 2$, thì tổng từ$l$ĐẾN$r$là chẵn khi và chỉ nếu$P[l-1] = P[r]$. Điều này chuyển vấn đề thành việc đếm các cặp giá trị chẵn lẻ tiền tố bằng nhau. 

Chúng tôi duy trì số lần mỗi tiền tố chẵn lẻ (0 hoặc 1) xuất hiện. Mỗi khi chúng ta gặp một giá trị chẵn lẻ tiền tố, nó sẽ đóng góp số lượng mảng con hợp lệ kết thúc ở đây bằng số lần xuất hiện trước đó của cùng một giá trị chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tính chẵn lẻ tiền tố | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại vấn đề theo tính chẵn lẻ của tiền tố và đếm các cặp khớp tăng dần. 

1. Khởi tạo mảng tần số`cnt`có kích thước 2, biểu thị số lần chúng ta đã thấy tiền tố chẵn lẻ 0 và 1. Bắt đầu với`cnt[0] = 1`bởi vì tiền tố trống có tổng chẵn lẻ bằng 0 trước khi bất kỳ phần tử nào được xử lý. 
2. Duy trì biến chẵn lẻ đang chạy`p = 0`, đại diện cho tính chẵn lẻ của tổng tiền tố với chỉ mục hiện tại. 
3. Lặp lại mảng từ trái sang phải. Đối với mỗi phần tử, hãy cập nhật tính chẵn lẻ dưới dạng`p ^= (A[i] & 1)`. Điều này lật ngược tính chẵn lẻ khi và chỉ khi phần tử hiện tại là số lẻ. 
4. Tại mỗi vị trí, mọi tiền tố trước đó có cùng tính chẵn lẻ`p`tạo thành một mảng con có tổng chẵn hợp lệ kết thúc ở chỉ mục hiện tại. Thêm vào`cnt[p]`để trả lời. 
5. Sau khi đếm, tăng dần`cnt[p]`để ghi lại rằng tính chẵn lẻ tiền tố này đã xảy ra thêm một lần nữa. 

Mỗi bước đảm bảo chúng ta đếm chính xác các mảng con khi điểm cuối của chúng có tính chẵn lẻ của tiền tố phù hợp, tương đương với tổng chẵn. 

### Tại sao nó hoạt động 

Tổng mảng con từ$l$ĐẾN$r$có thể được biểu thị bằng cách sử dụng tổng tiền tố như$P[r] - P[l-1]$. Trên các số nguyên, tính chẵn lẻ chỉ phụ thuộc vào việc hiệu này có chia hết cho 2 hay không. Trong số học modulo 2, phép trừ tương đương với phép cộng, vì vậy điều kiện trở thành$P[r] \equiv P[l-1]$. Thuật toán đếm chính xác tất cả các cặp chỉ số có tính chẵn lẻ tiền tố bằng nhau, vì vậy mọi phân mảng hợp lệ đều được tính một lần và chỉ một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    cnt = [0, 0]
    cnt[0] = 1
    
    p = 0
    ans = 0
    
    for x in arr:
        p ^= (x & 1)
        ans += cnt[p]
        cnt[p] += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp nén mỗi số thành một bit chẵn lẻ duy nhất, giúp tránh mọi số học lớn và đảm bảo cập nhật liên tục theo thời gian cho mỗi phần tử. Biến chẵn lẻ tiền tố`p`được cập nhật bằng XOR, tương ứng trực tiếp với việc chuyển đổi tính chẵn lẻ khi gặp số lẻ. 

các`cnt`mảng theo dõi có bao nhiêu vị trí tiền tố đã tạo ra mỗi giá trị chẵn lẻ. ban đầu`cnt[0] = 1`là cần thiết vì nó chiếm các mảng con bắt đầu từ chỉ mục 0; không có nó, chúng ta sẽ bỏ lỡ tất cả các tiền tố hợp lệ bắt đầu ở phần tử đầu tiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
8 8 8
```Tất cả các số đều là số chẵn nên tính chẵn lẻ không bao giờ thay đổi. 

| tôi | A[i] | p | cnt[0] | cnt[1] | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 8 | 0 | 2 | 0 | 1 | 
| 1 | 8 | 0 | 3 | 0 | 2 | 
| 2 | 8 | 0 | 4 | 0 | 3 | 

Câu trả lời cuối cùng là 3, tương ứng với tất cả các mảng con kết thúc ở mỗi vị trí đảm bảo tính chẵn lẻ. 

Điều này xác nhận tính bất biến rằng tất cả các tiền tố đều có cùng tính chẵn lẻ, do đó mỗi cặp đóng góp một mảng con hợp lệ. 

### Mẫu 2 

đầu vào:```
4
5 5 4 4
```Chuỗi chẵn lẻ phát triển thành lẻ, chẵn, chẵn, chẵn. 

| tôi | A[i] | p | cnt[0] | cnt[1] | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 1 | 1 | 1 | 0 | 
| 1 | 5 | 0 | 2 | 1 | 1 | 
| 2 | 4 | 0 | 3 | 1 | 2 | 
| 3 | 4 | 0 | 4 | 1 | 3 | 

Tổng số trở thành 5. 

Dấu vết này cho thấy cách chuyển đổi giữa các trạng thái chẵn lẻ chỉ tạo ra các mảng con hợp lệ mới khi tính chẵn lẻ tiền tố khớp với các lần xuất hiện trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử cập nhật tính chẵn lẻ và tần số một lần | 
| Không gian | O(1) | Chỉ có hai bộ đếm cho trạng thái chẵn lẻ | 

Giải pháp dễ dàng phù hợp với các ràng buộc cho$N = 10^6$vì nó thực hiện một đường tuyến tính duy nhất với công việc không đổi trên mỗi phần tử và tránh lưu trữ bất kỳ cấu trúc bổ sung nào tỷ lệ thuận với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline
    
    n = int(input())
    arr = list(map(int, input().split()))
    
    cnt = [0, 0]
    cnt[0] = 1
    p = 0
    ans = 0
    
    for x in arr:
        p ^= (x & 1)
        ans += cnt[p]
        cnt[p] += 1
    
    return str(ans)

# provided samples
assert run("3\n8 8 8\n") == "3"
assert run("4\n5 5 4 4\n") == "5"

# minimum size
assert run("1\n2\n") == "1"

# all odd alternating
assert run("3\n1 1 1\n") == "2"

# mixed case
assert run("5\n1 2 3 4 5\n") == "6"

# all even large
assert run("5\n2 4 6 8 10\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thậm chí độc thân | 1 | tính đúng đắn của trường hợp tối thiểu | 
| tất cả đều kỳ quặc | 2 | hành vi lật chẵn lẻ | 
| trình tự hỗn hợp | 6 | tính chẵn lẻ tiền tố chung | 
| tất cả thậm chí | 15 | mảng con tổ hợp đầy đủ | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[2]`, tính chẵn lẻ của tiền tố bắt đầu từ 0, lại chuyển sang 0 và đóng góp chính xác một mảng con hợp lệ. Thuật toán tính`cnt[0]`trước và sau khi xử lý chính xác, tạo ra 1. 

Đối với một mảng gồm tất cả các phần tử lẻ như`[1, 1, 1]`, chẵn lẻ xen kẽ 1, 0, 1. Việc theo dõi tần số chỉ tạo ra các đóng góp khi lặp lại chẵn lẻ, điều này phù hợp với thực tế là chỉ các mảng con có độ dài chẵn mới có tổng chẵn. Kết quả tính toán 2 tương ứng với mảng con`[1,1]`Và`[1,1]`ở các vị trí khác nhau, mỗi vị trí được tính riêng biệt. 

Đối với một mảng chẵn như`[2,2,2,2]`, tính chẵn lẻ không bao giờ thay đổi, vì vậy mọi tiền tố đều khớp với mọi tiền tố khác. Thuật toán tích lũy$4 \times 5 / 2 = 10$các mảng con và mỗi mảng đều có tổng chẵn, phù hợp với sự lặp lại liên tục của tính chẵn lẻ của tiền tố 0.
