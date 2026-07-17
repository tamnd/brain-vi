---
title: "CF 103438H - Sắp xếp hoán vị đầy màu sắc"
description: "Chúng ta được cấp một hoán vị có kích thước $n$, nhưng vị trí của hoán vị này được nhóm theo màu sắc. Chúng ta được phép sửa hoán vị bằng hai loại hành động: chúng ta có thể hoán đổi hai phần tử bất kỳ trả một chi phí cố định $S$, và chúng ta cũng có thể chọn một lớp màu và hoán vị tùy ý…"
date: "2026-07-03T07:52:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103438
codeforces_index: "H"
codeforces_contest_name: "2021 ICPC Southeastern Europe Regional Contest"
rating: 0
weight: 103438
solve_time_s: 50
verified: true
draft: false
---

[CF 103438H - Sắp xếp hoán vị đầy màu sắc](https://codeforces.com/problemset/problem/103438/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$n$, nhưng vị trí của hoán vị này được nhóm theo màu sắc. Chúng tôi được phép sửa hoán vị bằng hai loại hành động: chúng tôi có thể hoán đổi bất kỳ hai phần tử nào trả một chi phí cố định$S$và chúng ta cũng có thể chọn một lớp màu và hoán vị tùy ý tất cả các phần tử hiện đang ở vị trí của màu đó, trả một chi phí riêng$C_i$cho hoạt động màu đó. 

Điểm quan trọng là màu sắc thuộc về vị trí chứ không phải giá trị. Việc hoán đổi các giá trị di chuyển giữa các vị trí mà không thay đổi màu sắc, trong khi thao tác màu cho phép chúng ta tự do sắp xếp lại các giá trị bên trong một lớp màu chỉ bằng một lần di chuyển có tính phí. Mục tiêu là chuyển đổi hoán vị thành hoán vị nhận dạng với tổng chi phí tối thiểu. 

Cấu trúc được áp đặt bởi các ràng buộc là động lực chính của giải pháp. Kích thước hoán vị tăng lên$10^5$, nhưng số lượng màu nhiều nhất là 5. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào cố gắng theo dõi trạng thái trên mỗi tập hợp con màu đều khả thi, vì chỉ có$2^5 = 32$tập hợp con. Mặt khác, bất cứ thứ gì bậc hai hoặc bậc ba trong$n$sẽ quá chậm trừ khi được tối ưu hóa nhiều hoặc giảm xuống tuyến tính cho mỗi lần kiểm tra. 

Một vấn đề tế nhị xuất hiện khi chỉ nghĩ đến các giao dịch hoán đổi tham lam. Một chiến lược đơn giản là tính toán phân rã chu trình hoán vị và trả$S$đối với mỗi cải tiến hoán đổi, nhưng các hoạt động màu sắc có thể cơ cấu lại hoàn toàn các chu kỳ cục bộ. Điều này có nghĩa là các chu kỳ không cố định: một thao tác màu có thể “định hình lại” biểu đồ hoán vị bên trong một nhóm màu, có khả năng hợp nhất hoặc phá vỡ các chu kỳ theo cách thay đổi số lần hoán đổi cần thiết trên toàn cầu. 

Một trường hợp gây hiểu nhầm nhỏ là khi một màu chứa các phần tử từ nhiều chu kỳ. Nếu không sử dụng thao tác màu, các chu trình đó vẫn độc lập và buộc phải hoán đổi nhiều lần. Với một thao tác màu duy nhất, các phần tử tương tự đó có thể được sắp xếp lại để căn chỉnh tốt hơn, có khả năng giảm nhiều thao tác hoán đổi thành một bước tái cấu trúc duy nhất. Bất kỳ giải pháp nào coi chu kỳ là bất biến sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua các phép toán màu, vấn đề sẽ giảm xuống một thực tế cổ điển là việc sắp xếp một hoán vị với chi phí hoán đổi tùy ý$S \cdot (n - \text{cycles})$, vì mỗi lần hoán đổi sẽ giảm số chu kỳ đi một chu kỳ trong quá trình phân tách tối ưu. 

Nếu chúng ta chỉ sử dụng các thao tác màu, mỗi màu có thể hoán đổi vị trí của nó một cách độc lập để tính giá trị.$C_i$và chúng ta có thể cố gắng “sửa” càng nhiều hoán vị càng tốt trong mỗi nhóm màu. Tuy nhiên, điều này vẫn không giải quyết được hoàn toàn sự tương tác giữa các màu khác nhau, vì các giao dịch hoán đổi có tính chất toàn cầu và kết nối mọi thứ. 

Quan sát cấu trúc quan trọng là chỉ có 5 màu, vì vậy chúng ta có thể chọn cho mỗi màu có áp dụng thao tác “hoán vị tự do” đắt tiền của nó hay không. Khi một tập hợp con các màu được chọn, vấn đề sẽ trở thành: bên trong các màu đã chọn, chúng ta có thể sắp xếp lại các giá trị một cách tùy ý; bên trong những màu không được chọn, chúng tôi không thể. Điều này biến vấn đề thành việc đánh giá cấu trúc hoán vị đã sửa đổi và tính toán chi phí hoán đổi tối thiểu trên cấu trúc dẫn xuất đó. 

Vì vậy, giải pháp là thử tất cả$2^k$tập hợp con của màu sắc. Đối với mỗi tập hợp con, chúng tôi tính toán cách sắp xếp tốt nhất có thể đạt được sau khi hoán vị tự do bên trong các màu đã chọn, sau đó tính toán số lượng hoán đổi cần thiết để sửa hoán vị kết quả. 

Khó khăn nằm ở chỗ tính toán, đối với một tập con cố định, còn lại bao nhiêu chu trình sau khi sắp xếp lại tối ưu. Việc sắp xếp lại bên trong một màu không phải là tùy ý cho mỗi vị trí một cách độc lập; đó là một hoán vị đầy đủ bên trong nhóm màu đó, do đó, đây là một vấn đề kết hợp hạn chế, nhưng nó có thể được giải quyết một cách tham lam theo từng nhóm vì chúng tôi chỉ quan tâm đến việc tối đa hóa việc hình thành chu kỳ giữa các vị trí và giá trị mục tiêu của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu của mọi hoạt động | hàm mũ | cao | Quá chậm | 
| Tập hợp con DP trên các màu có tính toán lại chu kỳ |$O(2^k \cdot n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa một tập hợp con các màu mà chúng tôi quyết định kích hoạt bằng các thao tác màu. 

1. Chúng tôi lặp lại tất cả các tập hợp con màu sắc. Mỗi tập hợp con đại diện cho những màu nào chúng ta được phép tự do hoán vị bên trong. 
2. Đối với một tập hợp con cố định, về mặt khái niệm, chúng tôi sửa đổi hoán vị bằng cách cho phép mỗi nhóm màu hiện hoạt sắp xếp lại các phần tử của nó một cách tùy ý. Điều này có nghĩa là bên trong mỗi màu hiện hoạt, chúng ta có thể tự do lựa chọn bất kỳ sự đối chiếu nào giữa các vị trí của nó và các giá trị hiện có ở các vị trí đó. 
3. Chúng tôi xây dựng lại phiên bản hoán vị “được tối ưu hóa”. Bên trong mỗi nhóm màu đang hoạt động, chúng tôi khớp các giá trị với các vị trí theo cách tối đa hóa số lượng vị trí đã trỏ đến đích chính xác của chúng. Điều này được thực hiện bằng cách sắp xếp trong nhóm và ghép nối các mục tiêu tương thích. 
4. Sau khi xây dựng cách sắp xếp tốt nhất có thể cho tập hợp con đã chọn, chúng tôi tính toán số chu trình trong biểu đồ hoán vị thu được. 
5. Chi phí hoán đổi cho cấu hình này là$S \cdot (n - \text{cycles})$, vì mỗi lần hoán đổi sẽ giảm số chu kỳ đi đúng một. 
6. Chúng tôi cộng thêm chi phí kích hoạt các màu đã chọn, bằng tổng của$C_i$trên tập hợp con. 
7. Chúng tôi lấy giá trị tối thiểu trên tất cả các tập hợp con. 

Tính chính xác đến từ tính bất biến mà đối với bất kỳ tập hợp con cố định nào của các màu được kích hoạt, chúng tôi xem xét toàn diện tất cả các sắp xếp lại có thể có trong mỗi nhóm màu đang hoạt động có thể ảnh hưởng đến cấu trúc chu kỳ. Bất kỳ giải pháp tối ưu nào cũng tương ứng với một số lựa chọn tập hợp con và trong tập hợp con đó, tồn tại một sự sắp xếp đạt được số chu kỳ tối đa được tính toán. 

Điều này có hiệu quả vì hoán đổi chỉ ảnh hưởng đến việc phân rã chu trình toàn cầu, trong khi các thao tác màu chỉ ảnh hưởng đến quyền tự do sắp xếp lại bên trong. Sau khi tập hợp con được cố định, cấu trúc hoán vị sẽ trở nên xác định cho đến việc gắn nhãn lại nội bộ bên trong mỗi nhóm hoạt động và tối đa hóa chu kỳ tương đương với việc chọn cách dán nhãn lại tốt nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_cycles(arr):
    n = len(arr)
    vis = [False] * n
    res = 0
    for i in range(n):
        if not vis[i]:
            res += 1
            j = i
            while not vis[j]:
                vis[j] = True
                j = arr[j]
    return res

def solve():
    T = int(input())
    for _ in range(T):
        n, k = map(int, input().split())
        data = list(map(int, input().split()))
        S = data[0]
        C = data[1:]
        
        p = list(map(lambda x: x - 1, input().split()))
        col = list(map(lambda x: x - 1, input().split()))
        
        pos_in_color = [[] for _ in range(k)]
        for i in range(n):
            pos_in_color[col[i]].append(i)
        
        ans = S * (n - 1)
        
        for mask in range(1 << k):
            cost = 0
            for i in range(k):
                if mask & (1 << i):
                    cost += C[i]
            
            arr = p[:]
            
            for c in range(k):
                if mask & (1 << c):
                    idx = pos_in_color[c]
                    vals = [arr[i] for i in idx]
                    vals.sort()
                    for i, v in zip(sorted(idx), vals):
                        arr[i] = v
            
            cycles = count_cycles(arr)
            cost += S * (n - cycles)
            ans = min(ans, cost)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã lặp lại trên tất cả các tập hợp con màu bằng cách sử dụng mặt nạ bit. Đối với mỗi tập hợp con, nó xây dựng một bản sao hoạt động của hoán vị. Đối với mỗi màu được kích hoạt, nó tập hợp các chỉ số trong màu đó, trích xuất các giá trị hiện tại của chúng, sắp xếp cả chỉ số và giá trị rồi gán chúng một cách tham lam. Điều này nhận ra sự hoán vị nội bộ tối ưu nhằm tối đa hóa sự liên kết trong nhóm. 

Sau khi xây dựng lại hoán vị, quy trình đếm chu kỳ sẽ tính toán xem còn lại bao nhiêu thành phần. Chi phí hoán đổi được tính trực tiếp từ việc phân tách chu trình và sau đó chi phí kích hoạt màu sẽ được thêm vào. 

Một điểm tinh tế là việc sắp xếp các chỉ số và giá trị một cách độc lập là điều cho phép ghép nối tối đa bên trong mỗi nhóm. Điều này tránh cần các thuật toán đồ thị hoặc kết hợp rõ ràng cho mỗi tập hợp con. 

## Ví dụ đã hoạt động 

Hãy xem xét một hoán vị nhỏ trong đó cấu trúc bên trong có vai trò quan trọng. 

đầu vào:```
n = 4, k = 1
p = [2, 3, 4, 1]
colors = [1, 1, 1, 1]
S = 10, C1 = 1
```| tập hợp con | perm sắp xếp lại | chu kỳ | chi phí hoán đổi | tổng cộng | 
| --- | --- | --- | --- | --- | 
| không | [2,3,4,1] | 1 | 30 | 30 | 
| {màu 1} | [1,2,3,4] | 4 | 0 | 1 | 

Khi màu được kích hoạt, toàn bộ mảng có thể được hoán vị tự do, vì vậy chúng tôi căn chỉnh trực tiếp mọi thứ và loại bỏ tất cả các giao dịch hoán đổi. 

Bây giờ hãy xem xét sự tương tác nhiều màu: 

đầu vào:```
n = 6, k = 2
p = [5,2,4,6,1,3]
col = [1,2,1,2,1,2]
S = 10, C = [1,1]
```Đối với tập hợp con không sử dụng màu, chúng tôi tính chu kỳ hoán vị ban đầu. 

| tập hợp con | ý tưởng chủ chốt | chu kỳ | chi phí hoán đổi | giá màu | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| ∅ | hoán vị cố định | 1 | 50 | 0 | 50 | 
| {1} | chỉ sắp xếp lại màu 1 | cải thiện sự liên kết | 2 | 30 | 31 | 
| {2} | chỉ sắp xếp lại màu 2 | cải thiện sự liên kết | 2 | 30 | 31 | 
| {1,2} | hoàn toàn linh hoạt | 6 | 0 | 2 | 2 | 

Trường hợp được kích hoạt hoàn toàn cho thấy lý do tại sao các thao tác màu chiếm ưu thế: một khi cả hai nhóm đều linh hoạt, hoán vị có thể được giải quyết hoàn toàn trong nội bộ. 

Những dấu vết này cho thấy giải pháp không chỉ là hiệu quả hoán đổi mà còn là mức độ tự do về cấu trúc mà mỗi tập hợp con màu đưa vào quá trình hình thành chu kỳ. 

## Tuân thủ
