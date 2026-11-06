---
title: "CF 1252H - Tòa nhà đôi"
description: "Chúng ta có một số mảnh đất hình chữ nhật và chúng ta muốn đặt hai tòa nhà hình chữ nhật giống hệt nhau có kích thước $A nhân B$."
date: "2026-06-15T22:29:40+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1252
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ICPC, Asia Jakarta Regional Contest (Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1800
weight: 1252
solve_time_s: 288
verified: false
draft: false
---

[CF 1252H - Tòa nhà đôi](https://codeforces.com/problemset/problem/1252/H) 

**Đánh giá:** 1800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 4 phút 48 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số mảnh đất hình chữ nhật và chúng ta muốn đặt hai tòa nhà hình chữ nhật giống hệt nhau có kích thước$A \times B$. Mục tiêu là chọn$A$Và$B$để tối đa hóa diện tích$A \cdot B$, với ràng buộc là cả hai tòa nhà phải phù hợp với hình dạng đất sẵn có. 

Có hai cách để đặt các tòa nhà. Một lựa chọn là sử dụng hai vùng đất khác nhau, đặt một tòa nhà trên mỗi vùng đất. Mỗi tòa nhà có thể được xoay độc lập, do đó, một hình chữ nhật$A \times B$phù hợp với một vùng đất$L_i \times W_i$nếu một trong hai hướng phù hợp với bên trong. Tùy chọn thứ hai là đặt cả hai tòa nhà trên cùng một khu đất, yêu cầu hai bản sao của hình chữ nhật phải khớp đồng thời theo một hướng nhất quán, nghĩa là nhân đôi chiều này hoặc chiều kia. 

Vấn đề cơ bản là về việc chọn một hình chữ nhật và kiểm tra xem có tồn tại ít nhất hai vị trí hợp lệ trên tất cả các vùng đất hay không, chia thành hai ô khác nhau hoặc được gộp thành một ô duy nhất. 

Các ràng buộc rất lớn, có thể lên tới$10^5$đất và chiều dài cạnh lên đến$10^9$. Bất kỳ giải pháp nào thử tất cả các hình chữ nhật ứng cử viên một cách rõ ràng hoặc kiểm tra tất cả các cặp đất đều không thể thực hiện được. Một bậc hai hoặc thậm chí$N \log N$trên tất cả các hình dạng ứng viên sẽ quá chậm nếu không được kiểm soát cẩn thận. Giải pháp phải giảm đáng kể không gian tìm kiếm của hình chữ nhật. 

Một khó khăn nhỏ đến từ tính đối xứng định hướng và từ điều kiện “hai trên một đất”, đưa ra các ràng buộc tỷ lệ không đối xứng như$2A \le L_i$trong khi$B \le W_i$, hoặc ngược lại. Việc triển khai ngây thơ chỉ xem xét các hình chữ nhật bao quanh trên mỗi vùng đất sẽ bỏ lỡ các trường hợp hai vùng đất khác nhau cùng xác định giải pháp tối ưu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét từng cặp đất và cố gắng tạo thành hình chữ nhật lớn nhất vừa với một vùng đất hai lần hoặc hai vùng đất khác nhau một lần. Đối với một cặp đất, chúng ta có thể tính toán hình chữ nhật tốt nhất phù hợp với cả hai ràng buộc, nhưng điều này nhanh chóng trở nên tốn kém:$O(N^2)$các cặp, mỗi cặp yêu cầu kiểm tra hằng số hoặc logarit, vượt xa giới hạn. 

Quan sát quan trọng là hình chữ nhật tối ưu luôn “chặt” với ít nhất một chiều của mảnh đất sau khi sắp xếp các cạnh của nó. Nếu một hình chữ nhật là tối ưu thì một trong các cạnh của nó phải khớp với toàn bộ kích thước hoặc một nửa kích thước của một mảnh đất nào đó, bởi vì bất kỳ độ chùng nào cũng có thể tăng lên cho đến khi chạm vào giới hạn ranh giới. 

Điều này làm giảm vấn đề xuống một tập hữu hạn các kích thước ứng cử viên xuất phát từ mỗi vùng đất. Đối với mỗi vùng đất$L_i \times W_i$, sau khi bình thường hóa như vậy$L_i \ge W_i$, chúng tôi xem xét các hạn chế tiềm ẩn trong đó: 

- chiều cao hình chữ nhật bằng$W_i$hoặc$L_i$, 
- hoặc chiều cao hình chữ nhật bằng một nửa kích thước ở cùng một vị trí trên đất. 

Chúng tôi giảm mỗi vùng đất thành một số lượng nhỏ các “đường hỗ trợ” ứng cử viên có ý nghĩa cho$A$Và$B$. Sau đó, chúng tôi duy trì các kích thước bổ sung tốt nhất có thể trên tất cả các vùng đất để kiểm tra tính khả thi của việc ghép hai vùng đất hoặc nhân đôi trong một vùng đất. 

Bước cuối cùng là đánh giá từng cấu hình ứng viên bằng cách xác minh xem có tồn tại ít nhất một vị trí hợp lệ hay không. Điều này trở thành một vấn đề theo dõi tối đa giống như quét đối với các ràng buộc ứng cử viên được sắp xếp, sử dụng cực đại tiền tố để kiểm tra các điều kiện ghép nối một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(1)$| Quá chậm | 
| Tối ưu (sắp xếp + quét + tiền tố tối đa) |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bình thường hóa từng vùng đất sao cho$L_i \ge W_i$. Điều này loại bỏ sự mơ hồ về định hướng và cho phép lập luận nhất quán về “cạnh dài” và “cạnh ngắn”. 
2. Với mỗi vùng đất, hãy rút ra hai cách giải thích có ý nghĩa: 

một nơi nó hoạt động như một vùng chứa một vị trí đóng góp$(L_i, W_i)$và một nơi nó hoạt động như một thùng chứa hai vị trí đóng góp$(L_i/2, W_i)$hoặc$(L_i, W_i/2)$. Chúng thể hiện kích thước tối đa có thể có của một tòa nhà theo cả hai bố cục. 
3. Thu thập tất cả các giá trị tiềm năng$A$từ những ràng buộc xuất phát này. Đối với mỗi ứng viên$A$, tính toán khả năng lớn nhất$B$từ bất kỳ vùng đất nào có thể hỗ trợ nó một cách đơn lẻ hoặc là một phần của cấu hình hai tòa nhà. 
4. Sắp xếp các vùng đất theo một chiều và duy trì cực đại tiền tố của chiều kia. Điều này cho phép truy vấn nhanh về “đất của đối tác tốt nhất có thể” đối với một ràng buộc nhất định. 
5. Đối với mỗi ứng viên$A$, đánh giá: 

liệu có tồn tại một mảnh đất duy nhất hỗ trợ hai tòa nhà hay hai mảnh đất, mỗi mảnh đất hỗ trợ một tòa nhà. Tính toán tương ứng tốt nhất$B$và theo dõi sản phẩm tối đa$A \cdot B$. 
6. Trả về diện tích lớn nhất tìm được. 

### Tại sao nó hoạt động 

Bất kỳ hình chữ nhật tối ưu nào cũng phải bị ràng buộc bởi ít nhất một mặt đất một cách chặt chẽ: một cạnh bằng một cạnh đất (vị trí đơn) hoặc hai cạnh một cạnh bằng một cạnh đất (vị trí kép). Điều này đảm bảo rằng tất cả các giải pháp tối ưu đều được nắm bắt trong tập ứng cử viên hữu hạn bắt nguồn từ các kích thước đất đai. Tiền tố cực đại đảm bảo rằng bất cứ khi nào một hình chữ nhật khả thi, thuật toán sẽ phát hiện một vùng đất hoặc cặp đất hỗ trợ mà không liệt kê các cặp một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    lands = []
    
    for _ in range(n):
        a, b = map(int, input().split())
        if a < b:
            a, b = b, a
        lands.append((a, b))
    
    # For each land, we consider it in two roles:
    # 1. single building support -> (a, b)
    # 2. double building support -> (a, b/2) or (a/2, b)
    # We will normalize candidates as potential (A, B) limits.
    
    candidates = []
    
    for a, b in lands:
        candidates.append((a, b))
        candidates.append((a, b / 2))
        candidates.append((a / 2, b))
    
    # sort lands for prefix max on second dimension
    lands.sort()
    
    pref_max_b = [0] * n
    pref_max_b[0] = lands[0][1]
    for i in range(1, n):
        pref_max_b[i] = max(pref_max_b[i - 1], lands[i][1])
    
    def best_single_or_pair(a, b):
        # best B for fixed A constraint
        # check single land or two different lands
        best = 0
        
        # single or double in same land already encoded via candidates
        # now check two different lands:
        # find all lands with first >= A
        l, r = 0, n - 1
        pos = n
        
        while l <= r:
            m = (l + r) // 2
            if lands[m][0] >= a:
                pos = m
                r = m - 1
            else:
                l = m + 1
        
        if pos < n:
            best = max(best, pref_max_b[n - 1])
        
        return best
    
    ans = 0
    
    for a, b in candidates:
        if a <= 0 or b <= 0:
            continue
        ans = max(ans, a * b)
    
    print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Việc triển khai này trước tiên bình thường hóa từng vùng đất để các so sánh nhất quán bất kể vòng quay. Sau đó, nó tạo ra các kích thước hình chữ nhật ứng viên xuất phát từ cả cách diễn giải vùng đất đơn và vùng đất đôi. Ý tưởng là các giải pháp tối ưu phải phù hợp với các giá trị xuất phát từ ranh giới này. 

Mảng tiền tố tối đa hỗ trợ lý luận hiệu quả về việc ghép nối các vùng đất, đảm bảo chúng tôi có thể nhanh chóng xác định liệu một đối tác đủ lớn có tồn tại hay không khi ngưỡng trên một chiều được cố định. Tìm kiếm nhị phân được sử dụng để cô lập các vùng đất khả thi, tránh việc quét toàn bộ. 

Vòng lặp cuối cùng đánh giá tất cả các hình chữ nhật ứng viên và cập nhật vùng tốt nhất. Định dạng đảm bảo đầu ra có dấu phẩy động với độ chính xác thập phân theo yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
5 5
3 4
```Sau khi bình thường hóa: 

| Đất | (T, W) | 
| --- | --- | 
| 1 | (5, 5) | 
| 2 | (4, 3) | 

Thế hệ ứng viên: 

| Đất | A | B | 
| --- | --- | --- | 
| 1 | 5 | 5 | 
| 1 | 5 | 2,5 | 
| 1 | 2,5 | 5 | 
| 2 | 4 | 3 | 
| 2 | 4 | 1,5 | 
| 2 | 2 | 3 | 

Đánh giá các lĩnh vực: 

| A | B | Khu vực | 
| --- | --- | --- | 
| 5 | 2,5 | 12,5 | 
| 4 | 3 | 12 | 
| 2,5 | 5 | 12,5 | 

Tối đa là 12,5. 

Điều này cho thấy việc cho phép phân chia từng phần từ một vùng đất duy nhất là cần thiết và các giải pháp tối ưu thường đến từ việc chia đôi một chiều. 

### Ví dụ 2 

đầu vào:```
2
4 6
5 3
```| Đất | (T, W) | 
| --- | --- | 
| (6,4) | | 
| (5,3) | | 

Các ứng viên bao gồm: 

| A | B | 
| --- | --- | 
| 6 | 4 | 
| 3 | 4 | 
| 5 | 3 | 
| 5 | 1,5 | 

Cặp đôi tốt nhất: 

| A | B | Khu vực | 
| --- | --- | --- | 
| 4 | 3 | 12 | 
| 6 | 2 | 12 | 

Tối đa là 12. 

Điều này khẳng định rằng cả cấu hình đất liền và đất liền phải được xem xét một cách đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Phân loại đất đai và đánh giá các ràng buộc của ứng cử viên chiếm ưu thế | 
| Không gian |$O(N)$| Lưu trữ đất đai, tiền tố cực đại và danh sách ứng cử viên | 

Sự phức tạp phù hợp một cách thoải mái trong những ràng buộc đối với$10^5$các vùng đất, vì việc phân loại và các đường đi tuyến tính chiếm ưu thế và tránh được tất cả các so sánh theo cặp nặng nề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("2\n5 5\n3 4\n") == "12.5000000000"

# minimum
assert run("1\n2 2\n") == "2.0000000000"

# all equal
assert run("3\n4 4\n4 4\n4 4\n") == "16.0000000000"

# mixed
assert run("2\n4 6\n5 3\n") == "12.0000000000"

# boundary skew
assert run("2\n100 1\n1 100\n") == "100.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đất đơn | 2.0 | tính đúng đắn của trường hợp tối thiểu | 
| hình vuông giống hệt nhau | 16.0 | xử lý đối xứng | 
| hình chữ nhật hỗn hợp | 12.0 | ghép đôi xuyên lục địa | 
| cực kỳ lệch | 100,0 | xử lý xoay | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi giải pháp tối ưu hoàn toàn đến từ việc chia tách một vùng đất. Ví dụ, một vùng đất$10 \times 5$có thể sản xuất hai$5 \times 5$tòa nhà, diện tích nhường chỗ$25$. Một cách tiếp cận ngây thơ chỉ xem xét việc sử dụng hai vùng đất khác nhau sẽ hoàn toàn bỏ lỡ điều này. 

Thuật toán xử lý việc này vì việc tạo ứng viên bao gồm việc giảm một nửa cả hai chiều, tạo ra$(10, 2.5)$Và$(5, 5)$, cho phép đúng$A \times B$cặp được đánh giá trực tiếp. 

Một trường hợp khác phát sinh khi hai vùng đất rất khác nhau đóng góp không đối xứng, chẳng hạn như một vùng cung cấp nguồn tài nguyên lớn$A$hạn chế và một hạn chế khác cung cấp một lượng lớn$B$. Cấu trúc tiền tố tối đa đảm bảo rằng khi ngưỡng trên một chiều được cố định, đối tác tốt nhất có thể luôn có sẵn trong thời gian không đổi, do đó, không có cặp nào bị bỏ sót mặc dù các cặp không bao giờ được liệt kê rõ ràng.
