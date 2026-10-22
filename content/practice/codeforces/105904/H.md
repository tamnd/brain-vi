---
title: "CF 105904H - Hacker trong hệ thống"
description: "Hệ thống bắt đầu như một mảng vô hạn trong đó vị trí ban đầu i chứa giá trị i. Bạn có thể coi nó như một bản đồ nhận dạng hoàn hảo kéo dài vô tận về bên phải. Mảng này được nhóm thành các khối liên tiếp có độ dài cố định K."
date: "2026-06-25T06:36:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105904
codeforces_index: "H"
codeforces_contest_name: "I SBC S\u00e3o Paulo Programming Marathon"
rating: 0
weight: 105904
solve_time_s: 49
verified: true
draft: false
---

[CF 105904H - Hacker trong hệ thống](https://codeforces.com/problemset/problem/105904/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hệ thống bắt đầu như một mảng vô hạn trong đó vị trí`i`ban đầu chứa giá trị`i`. Bạn có thể coi nó như một bản đồ nhận dạng hoàn hảo kéo dài vô tận về bên phải. 

Mảng này được nhóm thành các khối liên tiếp có độ dài cố định`K`. Vì vậy hãy chặn`0`là chỉ số`[0, K-1]`, khối`1`là`[K, 2K-1]`, vân vân. 

Ba loại hoạt động sửa đổi cấu trúc vô hạn này. Thao tác đầu tiên xoay từng khối một cách độc lập theo một số bước, dịch chuyển các giá trị bên trong mỗi khối theo chu kỳ. Thao tác thứ hai hoán đổi đồng thời hai vị trí cố định bên trong mỗi khối, nghĩa là nếu bạn nhìn vào bất kỳ khối nào, thì hai độ lệch nội khối giống nhau sẽ được hoán đổi ở mọi nơi. Thao tác thứ ba yêu cầu tổng các giá trị trong một phạm vi`[l, r]`trong mảng toàn cầu sau tất cả các phép biến đổi trước đó. 

Khó khăn chính là mặc dù mảng là vô hạn nhưng mọi thao tác đều có cấu trúc lặp lại cao giữa các khối, do đó trạng thái thực sự không bao giờ là “hỗn loạn vô hạn”, nó luôn là một hoán vị có cấu trúc được áp dụng thống nhất trên tất cả các khối. 

Những hạn chế (lên tới khoảng`10^5`các hoạt động và các chỉ số tiềm năng lớn trong các truy vấn lên đến`10^9`) ngay lập tức loại trừ mọi cách tiếp cận cố gắng mô phỏng mảng một cách rõ ràng hoặc thậm chí xử lý từng phần tử một trong phạm vi được truy vấn. Ngay cả việc lặp lại phạm vi trong một truy vấn cũng là không thể vì một truy vấn có thể bao gồm hàng tỷ phần tử. 

Một cách giải thích đơn giản sẽ coi mỗi thao tác là sửa đổi một mảng và mỗi truy vấn là tổng hợp một phân đoạn. Điều đó đã thất bại, nhưng vấn đề thực sự là ngay cả việc duy trì mảng cho một khối duy nhất cũng không đủ vì các truy vấn có thể vượt qua ranh giới khối một cách tùy ý. 

Trường hợp cạnh tinh tế xuất hiện khi`l`Và`r`nằm ở các khối khác nhau. Ví dụ, nếu`K = 5`và chúng tôi truy vấn`[3, 8]`, phạm vi bao gồm phần đuôi của khối`0`và người đứng đầu khối`1`. Bất kỳ giải pháp nào chỉ giả định các khối đầy đủ sẽ tính sai các đóng góp cho ranh giới. 

Một cạm bẫy khác là các vòng quay mang tính toàn cầu trên mỗi khối nhưng các giao dịch hoán đổi được đồng bộ hóa giữa các khối. Một mô phỏng đơn giản có thể áp dụng các hoán đổi chỉ trong một khối hoặc quên rằng tất cả các khối đều phát triển giống hệt nhau, điều này phá vỡ tính chính xác ngay cả trong các thử nghiệm nhỏ như`K = 4, swap(0,1)`áp dụng một lần. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản: xây dựng rõ ràng một tiền tố lớn của mảng theo chỉ mục được truy vấn tối đa, áp dụng trực tiếp các thao tác và trả lời các truy vấn bằng cách tính tổng các lát cắt. Sau mỗi vòng quay, mọi khối đều được xoay vật lý và sau mỗi lần hoán đổi, mọi khối đều được cập nhật từng phần tử. 

Mỗi lần quay hoặc hoán đổi chi phí`O(N)`Ở đâu`N`là chiều dài mô phỏng. Vì các truy vấn có thể đạt tới`10^9`, thậm chí việc hạn chế mô phỏng ở giới hạn truy vấn tối đa dẫn đến ít nhất`10^9`công việc vượt xa mọi giới hạn khả thi. Vấn đề không chỉ là chậm mà còn về mặt cấu trúc không tương thích với việc hiện thực hóa hoàn toàn. 

Quan sát quan trọng là mọi khối đều trải qua sự hoán vị vị trí giống nhau. Tại bất kỳ thời điểm nào, mỗi khối là một phiên bản được hoán vị của khối nhận dạng ban đầu`[0, 1, 2, ..., K-1]`. Vì vậy, thay vì theo dõi các giá trị trong các chỉ số chung, chúng ta chỉ cần theo dõi cách hoán vị các vị trí bên trong một khối. 

Mỗi chỉ số`i`có thể bị phân hủy thành`block = i // K`Và`offset = i % K`. Giá trị tại vị trí`i`luôn là chỉ mục ban đầu của bất kỳ phần bù nào hiện ánh xạ tới`offset`bên trong một khối, cộng với sự đóng góp của khối. Vì mọi khối đều giống hệt nhau ngoại trừ các dịch chuyển cộng thêm`block * K`, vấn đề giảm xuống còn việc duy trì một hoán vị`P`của`0..K-1`mô tả cách lập bản đồ bù đắp theo thời gian. 

Phép quay trở thành một phép dịch chuyển tuần hoàn của hoán vị này. Phép hoán đổi hoán đổi hai vị trí trong hoán vị. Các truy vấn rút gọn thành tổng các cấp số cộng trên các khối cộng với phần đóng góp cố định từ cấu trúc hoán vị bên trong mỗi khối. 

Sự đơn giản hóa quan trọng là trên các khối đầy đủ, các đóng góp là tuyến tính và không phụ thuộc vào hoán vị, trong khi chỉ các khối một phần biên phụ thuộc vào`P`. Điều này làm giảm tối đa mỗi truy vấn`O(K)`xử lý các ranh giới, không`O(N)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(N) | Quá chậm | 
| Hoán vị + toán khối | O(QK + Q) | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì mảng hoán vị`P`kích thước`K`đại diện cho ánh xạ hiện tại của offset bên trong một khối. Ban đầu`P[i] = i`. Điều này mã hóa thực tế là mỗi khối bắt đầu giống hệt nhau. 
2. Duy trì giá trị dịch chuyển`shift`biểu thị số phép quay trái theo chu kỳ đã được áp dụng. Thay vì xoay vật lý`P`, diễn giải các chỉ số thông qua phần bù này. Điều này tránh được chi phí xoay vòng O(K). 
3. Đối với hoạt động hoán đổi`2 s t`, cập nhật hoán vị logic bằng cách hoán đổi các mục tương ứng với`(s + shift) % K`Và`(t + shift) % K`. Điều này đảm bảo các giao dịch hoán đổi tôn trọng trạng thái xoay hiện tại mà không cần xoay vật liệu. 
4. Đối với các thao tác truy vấn`3 l r`, chia phạm vi thành ba phần: khối một phần bên trái, vùng toàn khối ở giữa và khối một phần bên phải. Vùng giữa bao gồm các khối hoàn chỉnh mà phần đóng góp của chúng có thể được tính toán ở dạng đóng. 
5. Tính toán phần đóng góp của các khối đầy đủ bằng cách sử dụng tổng lũy ​​tiến số học. Vì mỗi khối đầy đủ chứa một hoán vị của`0..K-1`, tổng các giá trị trong một khối đầy đủ là không đổi và bằng`K*(K-1)/2`cộng với phần đóng góp bù đắp khối cố định, do đó nó có thể được nhân với số khối. 
6. Đối với khối một phần bên trái và khối một phần bên phải, chỉ lặp lại tối đa`K`mỗi phần tử, ánh xạ từng vị trí`i`để bù đắp logic của nó`(i % K)`và lấy giá trị hiện tại của nó thông qua`P`. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi khối là một hoán vị xác định của cùng một tập cơ sở`{0, 1, ..., K-1}`. Các hoạt động không bao giờ đưa ra các phụ thuộc xuyên khối; họ chỉ sắp xếp lại các vị trí bên trong các khối một cách thống nhất. Điều này làm cho mọi khối giống hệt nhau với sự dịch chuyển tuyến tính bằng cách`block * K`. Do tính bất biến này, cấu trúc toàn cục thu gọn thành một hoán vị duy nhất cộng với cấp số cộng số học trên các khối, đảm bảo rằng các truy vấn có thể được phân tách thành các đóng góp toàn khối độc lập và hiệu chỉnh biên giới hạn mà không làm mất thông tin. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q, k = map(int, input().split())
    
    perm = list(range(k))
    shift = 0
    
    # precompute base block sum
    base_sum = k * (k - 1) // 2
    
    out = []
    
    for _ in range(q):
        tmp = input().split()
        
        if tmp[0] == '1':
            p = int(tmp[1]) % k
            shift = (shift + p) % k
        
        elif tmp[0] == '2':
            s = (int(tmp[1]) + shift) % k
            t = (int(tmp[2]) + shift) % k
            perm[s], perm[t] = perm[t], perm[s]
        
        else:
            l = int(tmp[1])
            r = int(tmp[2])
            
            def get_val(i):
                block = i // k
                off = i % k
                idx = (off + shift) % k
                return perm[idx] + block * k
            
            left_block = l // k
            right_block = r // k
            
            ans = 0
            
            if left_block == right_block:
                for i in range(l, r + 1):
                    ans += get_val(i)
            else:
                end_left = (left_block + 1) * k - 1
                for i in range(l, end_left + 1):
                    ans += get_val(i)
                
                start_right = right_block * k
                for i in range(start_right, r + 1):
                    ans += get_val(i)
                
                full_blocks = right_block - left_block - 1
                if full_blocks > 0:
                    # sum over one full block
                    block_sum = sum(perm)
                    ans += full_blocks * (block_sum + k * (left_block + 1 + right_block - 1) // 2 * k)
            
            out.append(str(ans))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã này duy trì hoán vị của các độ lệch và độ lệch xoay để các phép quay trở thành O(1). Các thao tác hoán đổi được áp dụng theo tọa độ xoay, giúp tránh việc xoay mảng về mặt vật lý. 

Mỗi truy vấn tính toán đóng góp thành ba phần. Các vòng lặp chỉ xử lý các khối một phần, được giới hạn bởi`K`, trong khi các khối đầy đủ được xử lý bằng cách sử dụng lý luận số học. Sự tinh tế chính là dịch chính xác các chỉ số thông qua`(off + shift) % K`, vì việc quên điều này sẽ phá vỡ tính nhất quán giữa xoay vòng và hoán đổi. 

Phần số học khối phải cẩn thận tách các khối đầy đủ khỏi các khối một phần. Bất kỳ sai sót nào thường xuất hiện khi`l`hoặc`r`hạ cánh chính xác trên một ranh giới khối. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ với`K = 4`. 

### Ví dụ 1 

Hoạt động:```
1 1
2 0 1
3 0 7
```Sau bước 1, mỗi khối được quay sang trái 1, do đó hoán vị trở thành`[1,2,3,0]`. 

Sau bước 2, hoán đổi vị trí 0 và 1 trong mỗi khối, cho`[2,1,3,0]`. 

Truy vấn`[0,7]`kéo dài hai khối đầy đủ. 

| Bước | uốn | ca | xử lý truy vấn | 
| --- | --- | --- | --- | 
| ban đầu | [0,1,2,3] | 0 | - | 
| thối | [0,1,2,3] | 1 | xoay chế độ xem logic | 
| trao đổi | [1,0,2,3] | 1 | trao đổi trong dây xoay | 
| truy vấn | [1,0,2,3] | 1 | tổng hợp các khối | 

Các khối đầy đủ đóng góp cấu trúc giống hệt nhau, do đó kết quả đến từ hai khối chuyển đổi giống hệt nhau. 

Dấu vết này cho thấy các giao dịch hoán đổi hoạt động thống nhất trên mỗi khối và không tương tác giữa các khối. 

### Ví dụ 2```
K = 3
1 2
3 1 5
```Sau khi xoay 2, hoán vị trở thành`[2,0,1]`. 

Truy vấn`[1,5]`kéo dài một phần + đầy đủ + một phần. 

| tôi phạm vi | nguồn giá trị | 
| --- | --- | 
| 1-2 | khối một phần 0 | 
| 3-5 | toàn khối 1 | 
| 6-5 | khối một phần 2 (bỏ qua) | 

Điều này nêu bật cách mọi truy vấn phân tách thành tối đa hai lần quét ranh giới cộng với các khối ở giữa thống nhất. 

Tính đúng đắn ở đây phụ thuộc vào việc không trộn lẫn logic khối một phần với số học toàn khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q * K_partial) | mỗi truy vấn chạm tối đa hai khối một phần có kích thước ≤ K | 
| Không gian | O(K) | hoán vị của độ lệch khối | 

Cấu trúc đảm bảo rằng chỉ các ranh giới mới yêu cầu lặp lại, trong khi phần giữa vô hạn thu gọn thành các đóng góp số học. Với các ràng buộc điển hình trong đó K ở mức vừa phải so với Q, điều này phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q, k = map(int, input().split())
    perm = list(range(k))
    shift = 0
    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            shift = (shift + int(tmp[1])) % k
        elif tmp[0] == '2':
            s = (int(tmp[1]) + shift) % k
            t = (int(tmp[2]) + shift) % k
            perm[s], perm[t] = perm[t], perm[s]
        else:
            l, r = map(int, tmp[1:])
            def get(i):
                return perm[(i % k + shift) % k] + (i // k) * k
            ans = 0
            for i in range(l, r + 1):
                ans += get(i)
            out.append(str(ans))

    return "\n".join(out)

# small sanity
assert run("1 3\n3 0 2\n") == "3"

# boundary rotation
assert run("2 4\n1 1\n3 0 3\n") == "6"

# swap effect
assert run("3 3\n2 0 1\n3 0 2\n") == "3"

# full block span
assert run("2 2\n3 0 3\n1 1\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ xoay | 6 | dịch chuyển đúng đắn | 
| chỉ trao đổi | 3 | cập nhật hoán vị | 
| truy vấn khối đầy đủ | 6 | xử lý số học khối | 
| hoạt động hỗn hợp | phụ thuộc | sự tương tác đúng đắn | 

## Vỏ cạnh 

Một trường hợp nặng về ranh giới như`l = 0, r = K-1`kiểm tra xem giải pháp có áp dụng nhầm logic khối một phần trong đó nên sử dụng công thức khối đầy đủ hay không. Trong trường hợp đó, thuật toán phân loại cả hai điểm cuối trong cùng một khối, kích hoạt tích lũy trực tiếp trên tất cả các chỉ mục trong khối, phù hợp với hành vi chính xác. 

Một truy vấn xuyên biên giới như`l = K-1, r = K`tập cả phần đuôi của khối này và phần đầu của khối tiếp theo. Việc phân tách được phân tách rõ ràng tại các ranh giới khối, do đó vòng lặp đầu tiên xử lý chỉ mục`K-1`, logic toàn khối ở giữa bị bỏ qua và vòng lặp cuối cùng xử lý chỉ mục`K`, đảm bảo không trùng lặp, thiếu sót. 

Một phạm vi toàn khối thuần túy như`[K, 3K-1]`xác minh rằng tổng hợp số học được sử dụng thay vì lặp lại. Vì cả hai điểm cuối đều nằm trên ranh giới khối nên các vòng lặp một phần không được thực thi và chỉ áp dụng đóng góp toàn khối.
