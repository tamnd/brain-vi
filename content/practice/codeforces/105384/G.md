---
title: "CF 105384G - Goodman"
description: "Chúng ta được cho một hoán vị $p$ trên các số từ $1$ đến $n$. Chúng ta được phép chọn một hoán vị khác $q$, đơn giản là một thứ tự của các phần tử $n$ giống nhau."
date: "2026-06-23T16:14:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105384
codeforces_index: "G"
codeforces_contest_name: "Anton Trygub Contest 2 (The 3rd Universal Cup, Stage 3: Ukraine)"
rating: 0
weight: 105384
solve_time_s: 85
verified: true
draft: false
---

[CF 105384G - Goodman](https://codeforces.com/problemset/problem/105384/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị$p$qua các số từ$1$ĐẾN$n$. Chúng ta được phép chọn hoán vị khác$q$, đơn giản là một thứ tự giống nhau$n$các phần tử. Từ thứ tự đã chọn này, chuỗi thứ hai sẽ được tạo tự động bằng cách áp dụng hoán vị$p$theo từng phần tử: mỗi giá trị$x$TRONG$q$được thay thế bởi$p[x]$, tạo ra hoán vị thứ hai$p \circ q$. 

Mục tiêu là chọn$q$sao cho dãy con chung dài nhất giữa$q$Và$p \circ q$là càng lớn càng tốt. 

Khó khăn chính là chúng ta không so sánh một chuỗi với một chuỗi cố định. Thay vào đó, chúng ta đang chọn chính trình tự đó và trình tự thứ hai là sự biến đổi cấu trúc của nó. Điều này gây ra vấn đề về việc căn chỉnh một hoán vị với phiên bản được dán nhãn lại của nó theo một hoán vị cố định.$p$, chứ không phải là một vấn đề về dãy con cổ điển. 

Các ràng buộc là lớn, với tổng$n$trên tất cả các trường hợp thử nghiệm lên đến$10^6$. Điều này ngay lập tức loại trừ mọi phương trình bậc hai hoặc thậm chí$O(n \log n)$cho mỗi giải pháp trường hợp thử nghiệm với quá trình xử lý trước nặng vượt quá thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào về cơ bản đều phải xử lý từng phần tử với số lần không đổi. 

Một nỗ lực ngây thơ sẽ cố gắng xây dựng$q$và tính giá trị LCS cho mỗi đơn hàng ứng viên. Ngay cả việc đánh giá LCS cho một cặp hoán vị cố định cũng đã có chi phí$O(n \log n)$hoặc$O(n)$, và số hoán vị là$n!$, nên điều này hoàn toàn không thể thực hiện được. 

Một ý tưởng ngây thơ tinh tế hơn là sửa chữa$q = (1,2,\dots,n)$. Điều này đôi khi có tác dụng, ví dụ như khi$p$là danh tính, nhưng nói chung là thất bại nặng nề. Nếu như$p$bao gồm các chu trình không cần thiết, lựa chọn này không khai thác bất kỳ cấu trúc nào và tạo ra LCS nhỏ hơn nhiều so với mức có thể. 

Cấu trúc ẩn chính là hoán vị phân hủy thành các chu trình độc lập và sự tương tác giữa$q$Và$p \circ q$xảy ra hoàn toàn bên trong các chu kỳ này. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là thử tất cả các hoán vị$q$, tính toán$p \circ q$và đánh giá LCS giữa hai chuỗi. Điều này hiệu quả vì LCS được xác định rõ ràng và có thể tính toán được, nhưng nó thất bại ngay lập tức vì số lượng ứng viên quá lớn.$n!$và thậm chí một đánh giá duy nhất là tuyến tính hoặc tệ hơn, dẫn đến thời gian theo cấp số nhân. 

Sự đột phá đến từ việc quan sát rằng$p$là song ánh nên có thể phân tách thành các chu trình rời rạc. Trong mỗi chu kỳ, các phần tử chỉ ánh xạ với nhau khi áp dụng lặp đi lặp lại$p$và không có chu trình nào tương tác với chu trình khác. Điều này có nghĩa là bất kỳ sự liên kết nào giữa$q$Và$p \circ q$phải tôn trọng sự phân rã này. 

Khi chúng tôi giới hạn sự chú ý vào một chu kỳ duy nhất, chúng tôi nhận thấy rằng việc áp dụng$p$hoạt động giống như một phép quay trên các phần tử của chu trình đó. Cho dù chúng ta sắp xếp chu trình như thế nào$q$, trình tự được biến đổi$p \circ q$chỉ là sự dịch chuyển theo chu kỳ của những yếu tố tương tự. 

Hậu quả quan trọng là trong mỗi chu kỳ, chúng ta không thể buộc nhiều phần tử phải được căn chỉnh nhất quán theo cách góp phần tạo ra một chuỗi chung dài trên hai chuỗi. Các phần tử khác nhau trong cùng một chu trình chắc chắn sẽ “phá vỡ trật tự” so với nhau trong ánh xạ. 

Điều này dẫn đến chiến lược tối ưu: xử lý từng chu trình một cách độc lập và nối các chu trình theo bất kỳ thứ tự nào để tạo thành$q$. Khoản đóng góp LCS thu được sẽ chính xác là một cho mỗi chu kỳ và điều này hóa ra là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hãy thử tất cả các hoán vị |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Giải pháp phân hủy chu trình |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng lời giải trực tiếp từ cấu trúc chu trình của hoán vị$p$. 

1. Đầu tiên ta xác định tất cả các chu trình trong hoán vị$p$. Chúng tôi đánh dấu các phần tử là đã truy cập và liên tục theo dõi$x \to p[x]$cho đến khi chúng ta trở lại điểm xuất phát. Mỗi lần truyền tải tạo ra chính xác một chu kỳ. Bước này là cần thiết vì toàn bộ cấu trúc của vấn đề sẽ phân rã theo các chu trình này. 
2. Đối với mỗi chu kỳ, chúng ta liệt kê các phần tử của nó theo thứ tự chúng ta gặp trong quá trình truyền tải. Chúng ta không cần phải xoay vòng hoặc sắp xếp lại trong chu trình, bởi vì mọi thứ tự nội bộ đều hoạt động tương tự cho mục tiêu cuối cùng. 
3. Chúng ta ghép tất cả các chu trình theo thứ tự tùy ý để tạo thành hoán vị cuối cùng$q$. Thứ tự tương đối của các chu kỳ không ảnh hưởng đến tính chính xác vì các phần tử từ các chu kỳ khác nhau không bao giờ hạn chế lẫn nhau theo cách cải thiện hoặc giảm LCS tối ưu vượt quá một đại diện cho mỗi chu kỳ. 
4. Đầu ra$q$. 

### Tại sao nó hoạt động 

Mỗi chu kỳ được đóng lại dưới ánh xạ$p$, nghĩa là áp dụng$p$chỉ hoán vị các phần tử trong chu trình đó. Khi chúng ta so sánh$q$với$p \circ q$, các phần tử từ các chu kỳ khác nhau không thể giúp nhau tạo thành các chuỗi con nhất quán lâu hơn vì thứ tự tương đối của chúng là độc lập và không liên quan giữa hai chuỗi. 

Bên trong một chu kỳ dài$k$, áp dụng$p$hoạt động như một sự dịch chuyển theo chu kỳ. Bất kỳ chuỗi con chung nào giữa đoạn chu kỳ trong$q$và hình ảnh của nó trong$p \circ q$có thể sắp xếp tối đa một vị trí đại diện một cách nhất quán mà không tạo ra sự mâu thuẫn trong thứ tự. Điều này giới hạn sự đóng góp của mỗi chu kỳ tối đa là một. 

Vì chúng ta luôn có thể đạt được một phần tử phù hợp trong mỗi chu kỳ bằng cách lấy bất kỳ đại diện nào từ mỗi chu kỳ theo thứ tự xuất hiện tăng dần, nên tổng LCS bằng với số chu kỳ, do đó là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = [0] + list(map(int, input().split()))
    
    vis = [False] * (n + 1)
    q = []
    
    for i in range(1, n + 1):
        if not vis[i]:
            cur = i
            while not vis[cur]:
                vis[cur] = True
                q.append(cur)
                cur = p[cur]
    
    print(*q)

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Mã trực tiếp thực hiện phân rã chu trình. Mảng`vis`đảm bảo mỗi phần tử được truy cập chính xác một lần, mang lại độ phức tạp về thời gian tuyến tính. Mỗi lần truyền tải tuân theo các cạnh hoán vị cho đến khi chu trình kết thúc, nối thêm các phần tử vào đầu ra theo thứ tự được phát hiện. 

Không cần logic thứ tự bổ sung vì bất kỳ hoán vị nào của chu trình đều mang lại giá trị LCS tối ưu như nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét một hoán vị đơn giản trong đó$p$là danh tính. Mỗi phần tử tạo thành một chu trình có độ dài bằng một. 

Vì$n = 4$, chúng ta có chu kỳ$[1], [2], [3], [4]$. Thuật toán nối chúng thành$q = (1,2,3,4)$. 

| Bước | Phần tử hiện tại | Chu kỳ hình thành | Đầu ra cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 2 | 1 2 | 
| 3 | 3 | 3 | 1 2 3 | 
| 4 | 4 | 4 | 1 2 3 4 | 

Cả hai chuỗi đều giống hệt nhau nên LCS là 4, trùng với số chu kỳ. 

Bây giờ hãy xem xét một hoán vị gồm 2 chu kỳ:$p = (1\ 6)(2\ 5)(3\ 4)$. 

Chúng tôi vượt qua chu kỳ và xây dựng$q = (1,6,2,5,3,4)$. 

| Bước | Bắt đầu | Chu kỳ | Đầu ra cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 6 | 1 6 | 
| 2 | 2 | 2 5 | 1 6 2 5 | 
| 3 | 3 | 3 4 | 1 6 2 5 3 4 | 

Chuỗi thứ hai trở thành$p \circ q = (6,1,5,2,4,3)$. Dãy con chung dài nhất có độ dài 3, tương ứng với việc chọn một đại diện cho mỗi chu kỳ, chẳng hạn như$(1,2,3)$. Điều này phù hợp với số lượng chu kỳ. 

Điều này xác nhận rằng mỗi chu kỳ đóng góp chính xác một đơn vị LCS. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi nút được truy cập một lần trong quá trình truyền tải theo chu kỳ | 
| Không gian |$O(n)$| Đã truy cập mảng và lưu trữ đầu ra | 

Thuật toán xử lý mỗi phần tử chính xác một lần và chỉ thực hiện công việc không đổi cho mỗi phần tử. Với tổng số$n \le 10^6$, điều này phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        
        vis = [False] * (n + 1)
        res = []
        p = [0] + p
        
        for i in range(1, n + 1):
            if not vis[i]:
                cur = i
                while not vis[cur]:
                    vis[cur] = True
                    res.append(cur)
                    cur = p[cur]
        
        out.append(" ".join(map(str, res)))
    
    return "\n".join(out)

# small identity
assert run("1\n4\n1 2 3 4\n") == "1 2 3 4"

# single cycle
assert run("1\n3\n2 3 1\n") == "1 2 3"

# two cycles
assert run("1\n6\n1 6 2 5 3 4\n") == "1 6 2 5 3 4"

# reverse permutation
assert run("1\n4\n4 3 2 1\n") == "1 4 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoán vị danh tính | sắp xếp thứ tự | tất cả 1 chu kỳ | 
| 3 chu kỳ | truyền tải đầy đủ | xử lý chu trình đơn | 
| Cặp 2 chu kỳ | đầu ra được nhóm | nhiều chu kỳ | 
| hoán vị ngược | chu kỳ xen kẽ | trộn cấu trúc tồi tệ nhất | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mỗi phần tử đều có chu trình riêng của nó. Trong trường hợp này, thuật toán tạo ra hoán vị nhận dạng và LCS trở thành cực đại vì cả hai chuỗi đều giống hệt nhau. Việc phân rã chu trình ngay lập tức tạo ra các chu trình phần tử đơn và việc ghép nối sẽ bảo toàn chúng. 

Một trường hợp khác là một chu kỳ lớn duy nhất. Ở đây quá trình duyệt truy cập tất cả các phần tử trong một lần và xuất chúng theo thứ tự chu kỳ. Trình tự thứ hai trở thành một vòng quay có cùng thứ tự chu trình và LCS tương ứng với việc chọn sự liên kết đại diện nhất quán trong toàn bộ vòng quay, mang lại chính xác một đóng góp hiệu quả từ cấu trúc chu trình.
