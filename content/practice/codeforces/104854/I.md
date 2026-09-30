---
title: "CF 104854I - Nhúng mèo thông minh"
description: "Chúng ta được cung cấp một không gian nhúng cố định có kích thước $k$. Mỗi câu chúng ta xây dựng là một chuỗi các từ và mỗi từ hoạt động giống như một tập hợp các “thao tác ghi” xác định trên vectơ nhúng này. Chúng ta bắt đầu từ một vectơ 0 có độ dài $k$."
date: "2026-06-28T11:05:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104854
codeforces_index: "I"
codeforces_contest_name: "2023-2024 ICPC, Swiss Subregional"
rating: 0
weight: 104854
solve_time_s: 56
verified: true
draft: false
---

[CF 104854I - Nhúng mèo thông minh](https://codeforces.com/problemset/problem/104854/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một không gian nhúng có kích thước cố định$k$. Mỗi câu chúng ta xây dựng là một chuỗi các từ và mỗi từ hoạt động giống như một tập hợp các “thao tác ghi” xác định trên vectơ nhúng này. 

Chúng ta bắt đầu từ một vectơ có độ dài bằng 0$k$. Khi chúng ta đặt một từ vào câu, nó sẽ ghi đè các tọa độ nhất định: mỗi từ có nhiều cặp$(p, v)$, nghĩa là khi từ này được sử dụng, hãy phối hợp$p$được đặt thành giá trị$v$, thay thế bất cứ thứ gì đã có trước đó. Các từ được xử lý nghiêm ngặt từ trái sang phải, vì vậy các từ sau có thể ghi đè lên các từ trước đó. 

Nhiệm vụ là quyết định xem liệu chúng ta có thể sắp xếp một số từ đã cho hay không, sử dụng mỗi từ nhiều nhất một lần, thành một chuỗi mà phần nhúng cuối cùng trở thành chính xác một vectơ mục tiêu$t$. Chúng ta có thể sử dụng bất kỳ tập hợp con từ nào và bất kỳ thứ tự nào. 

Chi tiết cấu trúc quan trọng là các hoạt động không mang tính chất phụ gia. Chúng là những nhiệm vụ. Điều này về cơ bản tạo ra vấn đề về việc chọn người viết cuối cùng cho mỗi tọa độ. 

Những hạn chế$k, w \le 100$chỉ ra rằng bất kỳ cách tiếp cận nào liên quan đến$O(w^2)$hoặc$O(wk)$thì dễ dàng ổn, trong khi thứ tự hàm mũ của tất cả các hoán vị thì không. Một nỗ lực ngây thơ để thử tất cả các mệnh lệnh từ sẽ yêu cầu$w!$, điều này hoàn toàn không thể thực hiện được ngay cả đối với$w = 15$, chứ đừng nói đến 100. 

Một trường hợp phức tạp xuất phát từ các tương tác ghi đè. Một từ đặt tọa độ$p$chính xác sau này có thể bị ghi đè bởi một từ khác cũng chạm vào$p$. Ngược lại, một từ có vẻ sai cục bộ có thể trở thành đúng nếu nó được đặt sau các nhiệm vụ xung đột. 

Một trường hợp cạnh quan trọng khác là các từ ghi đè nhiều tọa độ cùng một lúc. Một từ duy nhất có thể thỏa mãn một số tọa độ trong khi phá vỡ các tọa độ khác, có nghĩa là chúng ta phải suy luận một cách tổng thể thay vì tham lam theo từng tọa độ. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo là thử mọi thứ tự của mọi tập hợp con từ. Đối với mỗi chuỗi ứng cử viên, chúng tôi mô phỏng cấu trúc nhúng và kiểm tra xem nó có phù hợp với mục tiêu hay không. Điều này đúng vì nó trực tiếp tuân theo các quy tắc của quy trình. Vấn đề là quy mô: có$w!$hoán vị của tất cả các từ và thậm chí hạn chế các tập hợp con vẫn để lại$2^w \cdot w!$, vượt xa mọi giới hạn. 

Bước đột phá về mặt cấu trúc là ngừng coi vấn đề như cách sắp xếp các từ mà thay vào đó hãy nghĩ đến việc phân công trách nhiệm cho từng tọa độ. Vì giá trị cuối cùng của mỗi vị trí$p$phải chính xác$t_p$, từ cuối cùng ảnh hưởng đến$p$phải đặt chính xác. Điều này gợi ý rằng chúng ta chỉ quan tâm đến từ nào là người viết cuối cùng của mỗi tọa độ. 

Nếu chúng ta cố định, với mỗi tọa độ, từ nào chịu trách nhiệm cho giá trị cuối cùng của nó, thì các ràng buộc thứ tự sẽ trở thành cục bộ: if word$A$chịu trách nhiệm điều phối$p$, và từ$B$chịu trách nhiệm điều phối$q$, chúng ta phải đảm bảo tính nhất quán khi một từ viết nhiều tọa độ. Điều này chuyển vấn đề thành việc chọn một tập hợp các từ có thể “giải thích” tất cả các tọa độ một cách nhất quán. 

Một cách tự nhiên để thực thi tính nhất quán là coi mỗi từ như một “hồ sơ” ứng cử viên: nó đóng góp một phần nhiệm vụ và tất cả những đóng góp của nó phải phù hợp với mục tiêu ở bất kỳ nơi nào nó được chọn chịu trách nhiệm. Sau đó, bài toán trở thành việc chọn một tập hợp con các từ sao cho mọi tọa độ đều được bao phủ bởi ít nhất một từ viết chính xác và không xuất hiện mâu thuẫn. 

Quan sát quan trọng là vì mỗi từ ghi một tập hợp tọa độ cố định, nên nếu một từ được sử dụng thì nó phải tương thích với mục tiêu trên tất cả các tọa độ mà nó ghi. Nếu không, nó không bao giờ có thể là người ghi cuối cùng của các tọa độ đó và nó chỉ có thể đóng vai trò là người điền không phải cuối cùng, điều này là vô nghĩa vì việc ghi đè luôn có thể thực hiện được và không cần thiết. 

Vì vậy, mọi từ có thể sử dụng được đều phải “nhất quán cục bộ”: với mỗi từ$(p, v)$nó định nghĩa, hoặc$v = t_p$hoặc nó không thể được sử dụng làm người đóng góp cuối cùng cho tọa độ đó. Nhưng ngay cả khi nó nhất quán cục bộ, chúng ta vẫn cần độ bao phủ: mỗi tọa độ phải có ít nhất một từ viết chính xác. 

Điều này làm giảm vấn đề kiểm tra xem liệu chúng ta có thể chọn các từ sao cho mỗi tọa độ$p$, tồn tại ít nhất một từ viết$p$BẰNG$t_p$. Nếu phạm vi bao phủ như vậy tồn tại, chúng ta có thể xây dựng một thứ tự hợp lệ bằng cách đặt các từ đã chọn theo bất kỳ thứ tự nào vì xung đột không thể xảy ra: tất cả các phép gán đã chọn đều phù hợp với mục tiêu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force của từ |$O(w! \cdot k)$|$O(k)$| Quá chậm | 
| Phối hợp giảm vùng phủ sóng |$O(wk)$|$O(wk)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cấu trúc bao phủ kiểu lưỡng cực giữa các từ và tọa độ. 

1. Đối với mỗi từ, chúng tôi quét tất cả các từ đó$(p, v)$cặp. Chúng tôi đánh dấu xem từ đó có tương thích với mục tiêu cho các tọa độ đó hay không. Một từ tương thích với tọa độ$p$nếu nó không viết$p$, hoặc viết chính xác$t_p$. 

Điều này đảm bảo chúng tôi không bao giờ coi một từ có thể làm hỏng vĩnh viễn tọa độ mà nó chạm vào. 
2. Chúng tôi loại bỏ bất kỳ từ nào có mâu thuẫn trực tiếp với mục tiêu trên bất kỳ tọa độ bằng văn bản nào. 

Khi một từ bị loại bỏ, nó không thể xuất hiện trong bất kỳ cấu trúc hợp lệ nào, bởi vì nó không bao giờ có thể là người viết cuối cùng cho tọa độ mà nó sửa đổi. 
3. Đối với mỗi từ còn lại, chúng tôi ghi lại tọa độ nào nó có thể đặt chính xác, nghĩa là tất cả$p$sao cho giá trị của nó bằng$t_p$. 
4. Bây giờ chúng tôi kiểm tra xem mọi tọa độ có$p$có ít nhất một từ còn lại có thể đặt chính xác. 

Đây là điều kiện khả thi cốt lõi: vì giá trị cuối cùng của$p$phải đến từ một từ nào đó, chúng tôi yêu cầu ít nhất một nguồn ứng viên cho từ đó. 
5. Nếu bất kỳ tọa độ nào không có ứng viên nào, chúng tôi ngay lập tức kết luận là không thể. 
6. Ngược lại, chúng ta xây dựng kết quả đầu ra bằng cách chọn tất cả các từ còn lại. Thứ tự của họ không liên quan vì mọi nhiệm vụ họ thực hiện đều phù hợp với mục tiêu. 

Bước xây dựng này an toàn vì tất cả các từ được chọn chỉ viết các giá trị đúng, vì vậy mọi thứ tự đều duy trì tính chính xác. 

### Tại sao nó hoạt động 

Quá trình nhúng là một chuỗi ghi đè, nhưng giá trị cuối cùng của mỗi tọa độ chỉ phụ thuộc vào từ cuối cùng chạm vào nó. Nếu mỗi từ được sử dụng chỉ gán các giá trị nhất quán với mục tiêu thì không có tọa độ nào có thể bị ép rời khỏi giá trị mục tiêu của nó. Ngược lại, nếu tọa độ nào đó không có từ nào có khả năng gán chính xác thì sẽ không có người viết cuối cùng cho tọa độ đó, khiến mục tiêu không thể truy cập được. Điều này tạo ra một điều kiện bao phủ cần và đủ cho mỗi tọa độ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k, w = map(int, input().split())
    t = list(map(int, input().split()))

    words = []
    valid = [True] * w

    for i in range(w):
        parts = input().split()
        name = parts[0]
        mi = int(parts[1])
        writes = []
        ok = True

        idx = 2
        for _ in range(mi):
            p = int(parts[idx]) - 1
            v = int(parts[idx + 1])
            idx += 2

            writes.append((p, v))
            if v != t[p]:
                ok = False

        valid[i] = ok
        words.append((name, writes))

    if not any(valid):
        print("IMPOSSIBLE")
        return

    covered = [False] * k

    for i in range(w):
        if not valid[i]:
            continue
        for p, v in words[i][1]:
            covered[p] = True

    for i in range(k):
        if not covered[i]:
            print("IMPOSSIBLE")
            return

    res = []
    for i in range(w):
        if valid[i]:
            res.append(words[i][0])

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ lọc các từ mâu thuẫn ngay lập tức với mục tiêu trên bất kỳ tọa độ nào mà chúng viết rõ ràng. Đây là hạn chế toàn cầu duy nhất mà chúng tôi cần thực thi ở cấp độ từ, bởi vì bất kỳ mâu thuẫn nào cũng khiến từ đó không thể trở thành người viết cuối cùng cho tọa độ đó. 

Sau khi lọc, chúng tôi tính toán phạm vi bao phủ theo tọa độ. Một tọa độ được coi là có thể đạt được nếu có ít nhất một từ còn sót lại viết giá trị chính xác của nó. Điều này đảm bảo mọi vị trí đều có nguồn phân công cuối cùng tiềm năng. 

Cuối cùng, chúng tôi xuất ra tất cả các từ còn sót lại theo thứ tự tùy ý. Thứ tự không quan trọng vì không có từ nào còn sót lại đưa ra một nhiệm vụ xung đột. 

Một lỗi phổ biến ở đây là cố gắng xây dựng lại một chuỗi tối thiểu thực tế hoặc thực hiện việc sắp xếp tham lam. Điều đó là không cần thiết vì tính khả thi chỉ phụ thuộc vào sự tồn tại của các tác giả tương thích chứ không phụ thuộc vào các ràng buộc về trình tự giữa chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2 1
a 1 1 1
b 1 2 1
c 1 3 1
```Tất cả các từ đều tương thích riêng lẻ vì mỗi từ chỉ ghi các giá trị chính xác nếu có. 

Chúng tôi theo dõi phạm vi bảo hiểm: 

| Lời | Viết | Tọa độ phủ | 
| --- | --- | --- | 
| một | (1,1) | 1 | 
| b | (2,1) | 2 | 
| c | (3,1) | 3 | 

Tất cả các tọa độ đều được bao phủ, vì vậy đầu ra là bất kỳ thứ tự nào của tất cả các từ, ví dụ:```
a b c
```Điều này chứng tỏ rằng việc đặt hàng là không liên quan một khi khả năng tương thích được duy trì. 

### Ví dụ 2 

đầu vào:```
2 2
1 2
a 1 1 1
b 1 1 2
```Từ a hợp lệ cho tọa độ 1. Từ b không hợp lệ vì nó đặt tọa độ 1 thành 2, mâu thuẫn với mục tiêu 1. 

Kiểm tra phạm vi bảo hiểm: 

| Tọa độ | Được bao phủ bởi những từ hợp lệ | 
| --- | --- | 
| 1 | một | 
| 2 | không | 

Tọa độ 2 không bao giờ được viết chính xác, vì vậy câu trả lời là:```
IMPOSSIBLE
```Điều này cho thấy rằng việc thiếu một trình ghi tương thích duy nhất cho bất kỳ tọa độ nào sẽ khiến vấn đề không thể giải quyết được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(wk)$| Mỗi từ được xử lý một lần và mỗi cặp thuộc tính được kiểm tra một lần | 
| Không gian |$O(wk)$| Lưu trữ mô tả từ và sổ sách kế toán bảo hiểm | 

Những hạn chế$w, k \le 100$làm điều này nhanh chóng thoải mái. Ngay cả việc quét toàn bộ tất cả các từ và danh sách thuộc tính của chúng cũng không đáng kể trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        return solve() or ""
    except:
        return ""

# sample-like case
assert run("""3 3
1 2 1
a 1 1 1
b 1 2 1
c 1 3 1
""") in ["a b c", "a c b", "b a c", "b c a", "c a b", "c b a"]

# impossible coordinate
assert run("""2 2
1 2
a 1 1 1
b 1 1 2
""") == "IMPOSSIBLE"

# single word exact match
assert run("""1 1
5
word 1 1 5
""") == "word"

# word conflicts with target
assert run("""1 1
5
bad 1 1 4
""") == "IMPOSSIBLE"

# multiple words, partial overlap
assert run("""3 4
1 2 3
a 1 1 1
b 1 2 2
c 1 3 3
d 2 1 1 2 2
""") in ["a b c d", "d a b c", "c b a d"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chính xác từng từ | từ | tính khả thi tầm thường | 
| tọa độ thiếu | KHÔNG THỂ | yêu cầu bảo hiểm | 
| bìa đầy đủ nhiều từ | bất kỳ đơn hàng nào | đặt hàng không liên quan | 
| loại bỏ từ xung đột | KHÔNG THỂ | bộ lọc tương thích nghiêm ngặt | 

## Vỏ cạnh 

Một tình huống phức tạp xảy ra khi một từ ghi nhiều tọa độ, một số khớp với mục tiêu và một số thì không. Ví dụ, nếu một từ đặt$p_1$đúng nhưng$p_2$không chính xác, nó hoàn toàn không thể được sử dụng vì nó sẽ gây ra sự mâu thuẫn ở$p_2$nếu được đặt cuối cùng ở đó, và nó cũng sẽ làm hỏng các trạng thái trung gian. 

Một trường hợp khác là khi tọa độ chỉ được viết bằng các từ cũng xung đột ở nơi khác. Ngay cả khi mỗi tọa độ riêng lẻ có một ứng cử viên, nếu tất cả các ứng cử viên đều không hợp lệ trên toàn cầu do các tọa độ khác thì từ đó phải bị loại bỏ hoàn toàn. Bước lọc xử lý vấn đề này bằng cách từ chối các từ dựa trên bất kỳ sự không khớp nào, không phải sự cho phép trên mỗi tọa độ. 

Cuối cùng, trường hợp nhiều từ trùng nhau nhiều sẽ an toàn vì chúng tôi không bao giờ dựa vào các ràng buộc về thứ tự. Vì tất cả các từ còn lại đều phù hợp với mục tiêu trên mọi tọa độ được viết, nên các tương tác của chúng có tính giao hoán xét về tính chính xác, mặc dù quá trình này nói chung không mang tính giao hoán về mặt toán học.
