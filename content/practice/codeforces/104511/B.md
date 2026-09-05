---
title: "CF 104511B - Tiền của Bessie"
description: "Chúng ta đang cố gắng gán các đồng xu cho sáu con bò khác nhau sao cho mỗi con bò nhận được chính xác một đồng xu và tổng giá trị của tất cả các đồng xu được chỉ định bằng tổng mục tiêu $n$."
date: "2026-06-30T10:42:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104511
codeforces_index: "B"
codeforces_contest_name: "Lexington Informatics Tournament (LIT) 2023"
rating: 0
weight: 104511
solve_time_s: 77
verified: true
draft: false
---

[CF 104511B - Tiền của Bessie](https://codeforces.com/problemset/problem/104511/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang cố gắng gán đồng xu cho sáu con bò riêng biệt để mỗi con bò nhận được chính xác một đồng xu và tổng giá trị của tất cả các đồng xu được chỉ định bằng tổng mục tiêu$n$. Số tiền hiện có bị giới hạn theo mệnh giá: với mỗi giá trị từ 1 đến 5, Nông dân John có một số cố định$a_x$những đồng tiền có giá trị$x$. Nhiệm vụ không chỉ là quyết định tính khả thi mà là đếm xem có bao nhiêu nhiệm vụ riêng biệt tồn tại, trong đó hai nhiệm vụ khác nhau nếu ít nhất một con bò nhận được một đồng xu có giá trị khác. 

Cấu trúc về cơ bản là một vấn đề phân phối bị ràng buộc: chúng tôi đang chọn chính xác sáu đồng tiền từ một tập hợp các nguồn lực hạn chế, với việc đặt hàng rất quan trọng vì những con bò rất khác biệt. Ngay cả khi hai bài tập sử dụng cùng nhiều giá trị đồng xu, việc hoán đổi con bò nào nhận được đồng xu nào sẽ tạo ra một cấu hình hợp lệ khác. 

Các hạn chế là nhỏ:$n \le 30$, và mỗi$a_x \le 6$. Điều này ngay lập tức báo hiệu rằng việc sử dụng vũ lực đối với tất cả các khoản phân bổ là hợp lý vì cả số lượng bò và số lượng đồng xu đều rất nhỏ. Bất kỳ cách tiếp cận nào liệt kê các nhiệm vụ trên sáu vị trí có giới hạn phân nhánh sẽ chấm dứt nhanh chóng. Một giải pháp liên quan đến sự kết hợp hàm mũ hoặc lập trình động trên một không gian trạng thái nhỏ được mong đợi. 

Một số tình huống khó khăn quan trọng: 

Nếu tổng số xu quá ít, thậm chí bỏ qua các giá trị thì không thể chỉ định được sáu xu. Ví dụ, nếu tất cả$a_x = 0$, thì không có sự phân công nào tồn tại bất kể$n$, và câu trả lời phải bằng 0. 

Nếu có đủ số xu nhưng giá trị của chúng quá nhỏ hoặc quá lớn thì ràng buộc tổng vẫn có thể thất bại. Ví dụ: nếu tất cả các đồng xu có giá trị 1 thì tổng số tiền duy nhất có thể đạt được là 6, do đó, bất kỳ đồng xu nào cũng có giá trị 1.$n \ne 6$không mang lại cách nào. 

Cuối cùng, ràng buộc bội số là quan trọng. Ngay cả khi tồn tại một tập hợp sáu giá trị hợp lệ có tổng giá trị là$n$, chúng tôi phải đảm bảo không tính vượt quá số lượng xu hiện có. Việc đếm tổ hợp đơn giản mà bỏ qua nguồn cung hạn chế sẽ không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là gán từng đồng xu cho mỗi con bò. Ở mỗi con bò, chúng tôi chọn bất kỳ loại đồng xu có sẵn nào, giảm tính khả dụng của nó và tiếp tục đệ quy. Điều này mô hình hóa chính xác vấn đề vì nó tôn trọng cả ràng buộc tổng và ràng buộc nguồn cung hạn chế. Kết quả đơn giản là số phép gán lá hợp lệ có tổng bằng$n$. 

Tuy nhiên, hệ số phân nhánh lên tới năm lựa chọn cho mỗi con bò và chúng ta có sáu con bò. Trong trường hợp xấu nhất điều này dẫn đến$5^6 = 15625$nhiệm vụ vốn đã nhỏ, nhưng chúng ta cũng phải xem xét việc cắt bớt theo mức độ sẵn có của tiền xu. Ngay cả khi chúng ta bỏ qua việc cắt tỉa thì điều này cũng không đáng kể về mặt tính toán. 

Chúng ta có thể làm cho cấu trúc gọn gàng hơn bằng cách nhận thấy rằng vấn đề tương đương với việc lấp đầy sáu vị trí được gắn nhãn với các giá trị trong$[1,5]$, tùy thuộc vào các ràng buộc chung về số lần mỗi giá trị có thể được sử dụng và điều kiện tổng. Đây tự nhiên là một tìm kiếm độ sâu giới hạn trên một không gian trạng thái rất nhỏ. 

Một chế độ xem có cấu trúc hơn một chút là lập trình động đối với các con bò và số tiền còn lại, đồng thời theo dõi số lượng xu còn lại. Nhưng vì mỗi$a_x \le 6$, một DFS trực tiếp có ghi nhớ hoặc thậm chí không có ghi nhớ là đủ. 

Thông tin chi tiết quan trọng là không gian trạng thái rất nhỏ vì chỉ có sáu quyết định và năm lựa chọn cho mỗi quyết định, do đó, việc kiểm tra tính hợp lệ bằng vũ lực đã là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS trên 6 con bò |$O(5^6)$|$O(1)$hoặc$O(6)$đệ quy | Đã chấp nhận | 
| DP với số xu |$O(6 \cdot n \cdot \prod a_x)$|$O(n \cdot \prod a_x)$| Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng con bò, duy trì số lượng xu của mỗi giá trị vẫn còn và số tiền còn lại mà chúng tôi cần đạt được. 

1. Bắt đầu với tất cả số xu$a_1 \ldots a_5$và tổng mục tiêu$n$, và chúng ta đang ở chỉ số bò 0. Tổng còn lại ban đầu là$n$. Điều này xác định trạng thái ban đầu của tìm kiếm. 
2. Đối với con bò hiện tại, hãy thử gán từng giá trị đồng xu từ 1 đến 5, miễn là chúng ta vẫn còn ít nhất một đồng xu có giá trị đó. Bước này thực thi hạn chế cung cấp cục bộ. 
3. Nếu chúng ta gán một đồng tiền có giá trị$v$, chúng tôi giảm$a_v$và giảm số tiền còn lại đi$v$, sau đó lặp lại với con bò tiếp theo. Điều này đảm bảo rằng việc gán một phần luôn phản ánh đúng các tài nguyên còn lại. 
4. Nếu tại bất kỳ thời điểm nào số tiền còn lại trở nên âm, chúng tôi ngay lập tức ngừng khám phá nhánh đó. Không có cách nào để phục hồi vì tất cả số tiền còn lại đều dương. 
5. Khi đến con bò thứ sáu, chúng tôi kiểm tra xem số tiền còn lại có chính xác bằng 0 hay không. Nếu đúng như vậy, chúng tôi coi đây là một bài tập hợp lệ. 

Tính đúng đắn dựa trên thực tế là mỗi lần gán đồng xu cho những con bò đều tương ứng với chính xác một đường dẫn trong cây tìm kiếm này. Mỗi cấp độ chọn một đồng xu của con bò và mỗi lựa chọn được xác định duy nhất bởi cả giá trị đồng xu và tính sẵn có. Do đó, chúng tôi không bỏ lỡ các bài tập hợp lệ cũng như không tính hai lần bài tập tương tự trong quá trình tiến hóa trạng thái giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    cows = 6
    values = [1, 2, 3, 4, 5]
    
    sys.setrecursionlimit(10000)
    
    from functools import lru_cache
    
    @lru_cache(None)
    def dfs(i, c1, c2, c3, c4, c5, rem):
        if rem < 0:
            return 0
        if i == cows:
            return 1 if rem == 0 else 0
        
        res = 0
        counts = [c1, c2, c3, c4, c5]
        
        for v in range(5):
            if counts[v] > 0:
                nxt = counts[:]
                nxt[v] -= 1
                res += dfs(i + 1, nxt[0], nxt[1], nxt[2], nxt[3], nxt[4], rem - (v + 1))
        
        return res
    
    print(dfs(0, a[0], a[1], a[2], a[3], a[4], n))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp cấu trúc đệ quy được mô tả trước đó. Tiểu bang bao gồm cả chỉ số bò và số lượng tiền xu còn lại, vì tính sẵn có của tiền xu ảnh hưởng đến các lựa chọn trong tương lai. Số tiền còn lại được mang dưới dạng tham số để sớm thực thi ràng buộc mục tiêu. 

Việc ghi nhớ đảm bảo rằng các trạng thái lặp lại, có thể xảy ra khi các chuỗi lựa chọn khác nhau dẫn đến cùng một chỉ số bò và nhiều tập hợp còn lại, không được tính toán lại. Điều này giúp việc thực thi được nhanh chóng ngay cả khi đệ quy thô vốn đã nhỏ. 

Một sai lầm phổ biến là quên rằng các con bò là khác biệt, điều này sẽ dẫn đến việc chia theo giai thừa hoặc sử dụng số tổ hợp không chính xác. Ở đây chúng tôi xử lý rõ ràng từng vị trí của con bò theo thứ tự, do đó không cần chuẩn hóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
6 2 3 4 1
```Chúng tôi theo dõi một phần nhỏ hành vi của DFS. 

| Bước | Chỉ số bò | Số tiền còn lại | Lựa chọn tiền xu | Số còn lại | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 8 | bắt đầu | (6,2,3,4,1) | 
| 1 | 0 | 8 | lấy 1 | (5,2,3,4,1) | 
| 2 | 1 | 7 | lấy 1 | (4,2,3,4,1) | 
| 3 | 2 | 6 | lấy 3 | (4,2,2,4,1) | 
| 4 | 3 | 3 | lấy 1 | (3,2,2,4,1) | 
| 5 | 4 | 2 | lấy 1 | (2,2,2,4,1) | 
| 6 | 5 | 1 | lấy 1 | (1,2,2,4,1) | 

Nhánh này kết thúc không thành công vì sau tất cả sáu con bò, chúng ta vẫn còn tổng 1. Các nhánh khác đặt đồng tiền có giá trị cao hơn sớm hơn sẽ thành công và đệ quy tích lũy chính xác 21 phép gán hợp lệ. 

Dấu vết này cho thấy ràng buộc tổng còn lại lọc dần dần các phép gán từng phần không hợp lệ trước khi đạt đến độ sâu sáu. 

### Ví dụ 2 

đầu vào:```
6
6 0 0 0 0
```Ở đây chỉ có giá trị 1 đồng tiền tồn tại. 

| Bước | Chỉ số bò | Số tiền còn lại | Lựa chọn tiền xu | Số còn lại | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 6 | lấy 1 | (5,0,0,0,0) | 
| 1 | 1 | 5 | lấy 1 | (4,0,0,0,0) | 
| 2 | 2 | 4 | lấy 1 | (3,0,0,0,0) | 
| 3 | 3 | 3 | lấy 1 | (2,0,0,0,0) | 
| 4 | 4 | 2 | lấy 1 | (1,0,0,0,0) | 
| 5 | 5 | 1 | lấy 1 | (0,0,0,0,0) | 

Đây là con đường thành công duy nhất, xác nhận câu trả lời là 1. Bất kỳ sai lệch nào ngay lập tức dẫn đến số tiền còn lại âm hoặc tổng số tiền còn lại không khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(5^6)$| Sáu con bò, mỗi con thử tối đa năm loại tiền xu, được cắt tỉa nhiều theo tình trạng sẵn có | 
| Không gian |$O(5^5)$tiểu bang | Ghi nhớ chỉ số bò, số lượng còn lại và tổng | 

Tổng số trạng thái rất nhỏ vì cả độ sâu và giới hạn đồng xu đều bị giới hạn bởi các hằng số nhỏ. Ngay cả trong trường hợp xấu nhất, đệ quy chỉ khám phá vài chục nghìn trạng thái, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    # --- solution ---
    import sys
    input = sys.stdin.readline

    def solve():
        n = int(input().strip())
        a = list(map(int, input().split()))
        cows = 6

        from functools import lru_cache

        @lru_cache(None)
        def dfs(i, c1, c2, c3, c4, c5, rem):
            if rem < 0:
                return 0
            if i == cows:
                return 1 if rem == 0 else 0

            res = 0
            counts = [c1, c2, c3, c4, c5]
            for v in range(5):
                if counts[v] > 0:
                    nxt = counts[:]
                    nxt[v] -= 1
                    res += dfs(i+1, nxt[0], nxt[1], nxt[2], nxt[3], nxt[4], rem-(v+1))
            return res

        return dfs(0, a[0], a[1], a[2], a[3], a[4], n)

    return str(solve())

# provided sample
assert run("8\n6 2 3 4 1\n") == "21"

# minimum edge: impossible
assert run("10\n0 0 0 0 0\n") == "0"

# exact fill with ones only
assert run("6\n6 0 0 0 0\n") == "1"

# boundary mix
assert run("7\n6 6 6 6 6\n") > "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bằng 0 | 0 | không có sẵn nguồn lực | 
| tất cả những cái, tổng 6 | 1 | phân công bắt buộc chính xác | 
| tiền phong phú, số tiền nhỏ | tích cực | sự đúng đắn của vụ nổ tổ hợp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tổng số xu không đủ. Đối với đầu vào`n = 6, a = (0,0,0,0,0)`, DFS ngay lập tức nhận thấy rằng tại bò 0 không thể di chuyển được, do đó đệ quy trả về 0. Điều này xác nhận rằng việc thiếu tài nguyên sẽ chặn tất cả các đường dẫn một cách chính xác. 

Một trường hợp khác là khi tiền tồn tại nhưng không thể đạt được số tiền yêu cầu. Ví dụ,`n = 7, a = (6,0,0,0,0)`buộc tất cả các con bò phải lấy đồng xu giá trị 1, tạo ra tổng 6 ở cấp độ lá, không bao giờ đạt tới 7. Thuật toán đạt đến độ sâu sáu với tổng còn lại là 1 và từ chối tất cả các đường dẫn. 

Một trường hợp tinh vi cuối cùng là có quá nhiều tiền xu, nơi tồn tại nhiều sự kết hợp. Vì mỗi phép gán được coi là một chuỗi có thứ tự riêng biệt đối với các con bò, nên DFS sẽ đếm từng hoán vị một cách tự nhiên mà không cần hiệu chỉnh tổ hợp bổ sung, phù hợp với định nghĩa dự kiến ​​về tính khác biệt.
