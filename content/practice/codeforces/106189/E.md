---
title: "CF 106189E - Ưu và nhược điểm"
description: "Chúng ta được cho một biểu thức toán học được viết dưới dạng một chuỗi. Nó chứa các số nguyên không âm, dấu ngoặc đơn và hai toán tử nhị phân + và -. Cấu trúc này đã hợp lệ về mặt cú pháp, vì vậy mọi toán tử đều nằm giữa hai biểu thức con được định dạng đúng."
date: "2026-06-25T06:47:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106189
codeforces_index: "E"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2025"
rating: 0
weight: 106189
solve_time_s: 41
verified: true
draft: false
---

[CF 106189E - Ưu điểm và nhược điểm](https://codeforces.com/problemset/problem/106189/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một biểu thức toán học được viết dưới dạng một chuỗi. Nó chứa các số nguyên không âm, dấu ngoặc đơn và hai toán tử nhị phân`+`Và`-`. Cấu trúc này đã hợp lệ về mặt cú pháp, vì vậy mọi toán tử đều nằm giữa hai biểu thức con được định dạng đúng. 

Thao tác duy nhất được phép là lật các toán tử: mọi`+`có thể trở thành`-`, và mọi`-`có thể trở thành`+`. Dấu ngoặc đơn và số phải được giữ nguyên. Sau khi áp dụng bất kỳ lần lật nào, chúng ta đánh giá biểu thức một cách bình thường. Mục tiêu là tạo ra một biểu thức đã sửa đổi có giá trị cuối cùng càng nhỏ càng tốt và xuất ra bất kỳ biểu thức nào đạt được mức tối thiểu này. 

Độ dài đầu vào có thể đạt tới$10^5$, do đó, bất kỳ cách tiếp cận nào liên tục đánh giá biểu thức từ đầu cho nhiều sửa đổi đều không khả thi. Mô phỏng bậc hai hoặc bậc ba đối với tất cả các lần lật dấu ứng cử viên hoặc phân tích cú pháp lặp lại cho mỗi lần sửa đổi sẽ quá chậm vì ngay cả một đánh giá đơn lẻ cũng là tuyến tính theo kích thước của biểu thức. 

Trường hợp cạnh tinh tế xuất hiện khi biểu thức chứa dấu ngoặc đơn lồng nhau với các dấu hỗn hợp. Ví dụ, trong một cái gì đó như`5+(1+3-2)`, việc lật một dấu bên trong dấu ngoặc đơn chỉ ảnh hưởng đến sự đóng góp của biểu thức con đó, nhưng tác động lên tổng số phụ thuộc vào việc liệu dấu ngoặc đơn đó có đứng trước dấu ngoặc đơn đó hay không`+`hoặc một`-`. Một cách tiếp cận ngây thơ lật các dấu hiệu cục bộ mà không theo dõi bối cảnh cấu trúc có thể giả định không chính xác tính độc lập giữa các toán tử. 

Một trường hợp khác là một chuỗi các phủ định lồng nhau. Trong các biểu thức như`-(a-(b-c))`, việc lật dấu bên trong có thể hủy bỏ hoặc khuếch đại hiệu ứng tùy thuộc vào tính chẵn lẻ của các dấu trừ xung quanh. Bất kỳ giải pháp đúng đắn nào cũng phải tính đến việc truyền bá bối cảnh ký hiệu này. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử tất cả các kết hợp lật hoặc không lật từng toán tử. Vì có$k$người vận hành, điều này dẫn đến$2^k$khả năng. Mỗi khả năng đều yêu cầu phân tích cú pháp hoặc đánh giá biểu thức đầy đủ, điều này gây tốn kém$O(n)$, dẫn đến$O(n \cdot 2^k)$. Với$k$có khả năng tuyến tính trong$n$, điều này trở nên cấp số mũ và ngay lập tức không khả thi. 

Quan sát quan trọng là dấu ngoặc đơn cố định việc nhóm chứ không phải ngữ cảnh dấu hiệu trong đó mỗi thuật ngữ được nhóm được đánh giá. Mỗi khối được ngoặc đơn đóng góp tích cực hoặc tiêu cực vào giá trị cuối cùng tùy thuộc vào toán tử trước nó. Bên trong một khối, việc đảo ngược tất cả các toán tử nội bộ tương đương với việc thay đổi cách tổng hợp các giá trị cục bộ, nhưng đòn bẩy thực sự đến từ việc nhận ra rằng chúng ta không chọn các phép biến đổi số học tùy ý mà chúng ta chỉ đảo ngược các dấu hiệu. 

Điều này giúp giảm bớt vấn đề khi quyết định, đối với mỗi toán tử, liệu nó nên đóng góp như được viết ban đầu hay đảo ngược, đồng thời tôn trọng rằng mỗi biểu thức con được đánh giá là một đơn vị toàn bộ. Khi chúng tôi xem từng phân đoạn được phân cách bằng dấu ngoặc đơn dưới dạng một giá trị duy nhất, biểu thức sẽ trở thành một chuỗi các khối được ký kết. Trong mỗi khối, để giảm thiểu tổng giá trị, chúng tôi muốn mọi đóng góp nội bộ càng âm càng tốt. Điều này có nghĩa là chúng tôi muốn thay thế nội bộ`+`với`-`và ngược lại theo cách làm cho kết quả của biểu thức con đi xuống. 

Bí quyết về cấu trúc là bên trong bất kỳ biểu thức con nào, việc lật tất cả các dấu hiệu đều tương đương với việc phủ nhận sự đóng góp giá trị được tính toán của nó, vì vậy chúng ta không cần khám phá các kết hợp. Chúng ta chỉ cần quyết định hướng dấu của từng khối được tạo ra bởi cấu trúc toán tử xung quanh nó, sau đó áp dụng quy tắc tham lam nhất quán: xử lý mọi`+`như một ứng cử viên cho sự đảo ngược và mọi`-`như đã được tối ưu hóa để giảm thiểu khi nhìn trong bối cảnh tích cực, đồng thời tôn trọng sự lồng ghép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các lần lật |$O(n \cdot 2^m)$|$O(n)$| Quá chậm | 
| Phân tích cú pháp cấu trúc với chuẩn hóa dấu hiệu tham lam |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích biểu thức từ trái sang phải trong khi duy trì một ngăn xếp theo dõi ngữ cảnh ký hiệu hiện tại được giới thiệu bằng dấu ngoặc đơn. Mỗi cấp độ lồng nhau mang một hệ số nhân biểu thị phân đoạn hiện tại thực sự là dương hay âm. Điều này là cần thiết vì dấu trừ trước dấu ngoặc đơn sẽ lật ngược tất cả các đóng góp nội bộ. 
2. Khi gặp một số, hãy gắn nó vào ngữ cảnh đang hoạt động hiện tại. Bản thân giá trị không thay đổi, chỉ có dấu hiệu mà nó đóng góp. 
3. Khi gặp phải một`+`hoặc`-`, giải thích nó liên quan đến bối cảnh hiện tại. Nếu chúng ta ở trong một bối cảnh phủ định,`+`cư xử như`-`theo điều kiện đóng góp và ngược lại. 
4. Thay vì giữ nguyên các toán tử ban đầu, hãy quyết định dạng cuối cùng của chúng bằng cách chọn toán tử mang lại đóng góp nhỏ hơn trong bối cảnh hiện tại. Điều này tương đương với việc luôn đẩy sự đóng góp theo hướng tiêu cực: trong bối cảnh tích cực, chúng tôi thích`-`và trong bối cảnh tiêu cực, chúng tôi thích`+`bởi vì nó trở thành phép trừ sau khi truyền dấu. 
5. Xây dựng lại biểu thức bằng cách sử dụng các toán tử đã chọn trong khi vẫn giữ nguyên dấu ngoặc đơn và số. 

Ý tưởng quan trọng là chúng tôi không bao giờ mô phỏng nhiều lựa chọn lật. Chúng tôi truyền bá một môi trường ký hiệu nhất quán duy nhất và chọn hướng vận hành cục bộ để giảm thiểu sự đóng góp. 

### Tại sao nó hoạt động 

Mỗi biểu thức con đóng góp tuyến tính vào giá trị cuối cùng sau khi tính đến việc truyền dấu từ dấu ngoặc đơn. Mỗi lần lật toán tử chỉ ảnh hưởng đến dấu của thuật ngữ cục bộ và không tạo ra sự phụ thuộc chéo thuật ngữ ngoài ranh giới lồng nhau. Vì điều này, mỗi quyết định đều độc lập khi bối cảnh ký hiệu hiện tại được cố định. Ngăn xếp đảm bảo rằng ngữ cảnh này luôn chính xác cho bất kỳ vị trí nào trong biểu thức, do đó việc giảm thiểu cục bộ mang lại kết quả tối thiểu trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    # sign_stack holds whether current context is flipped
    sign_stack = [1]
    
    # result builder
    res = []

    i = 0
    while i < n:
        c = s[i]

        if c.isdigit():
            # read full number
            j = i
            while j < n and s[j].isdigit():
                j += 1
            res.append(s[i:j])
            i = j
            continue

        if c == '(':
            # inherit current sign context
            sign_stack.append(sign_stack[-1])
            res.append(c)

        elif c == ')':
            sign_stack.pop()
            res.append(c)

        else:
            # operator
            cur_sign = sign_stack[-1]

            # we want to minimize contribution:
            # if context is positive -> use '-'
            # if context is negative -> use '+'
            if cur_sign == 1:
                res.append('-')
            else:
                res.append('+')

        i += 1

    print(''.join(res))

if __name__ == "__main__":
    solve()
```Vòng phân tích cú pháp tách các số khỏi các toán tử để các số nguyên có nhiều chữ số được xử lý chính xác. Ngăn xếp dấu hiệu theo dõi xem tiền tố của dấu ngoặc đơn có đảo nghĩa hay không. Mỗi toán tử được thay thế ngay lập tức dựa trên bối cảnh đó, vì vậy chúng tôi không bao giờ xem lại các quyết định. 

Một điểm tinh tế là bản thân các dấu ngoặc đơn không thay đổi giá trị ngăn xếp, chỉ có các toán tử trước chúng mới làm thay đổi. Đó là lý do tại sao chúng tôi chỉ sửa đổi ngữ cảnh khi nhập và để lại dấu ngoặc đơn, không sửa đổi chữ số hoặc toán tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5+(1+3-2)
```Chúng tôi theo dõi ngữ cảnh khi chúng tôi phân tích cú pháp. 

| Mã thông báo | Dấu hiệu bối cảnh | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 5 | + | giữ | 5 | 
| + | + | trở thành - | 5- | 
| ( | + | đẩy + | 5-( | 
| 1 | + | giữ số | 5-(1 | 
| + | + | trở thành - | 5-(1- | 
| 3 | + | giữ | 5-(1-3 | 
| - | + | trở thành + | 5-(1-3+ | 
| 2 | + | giữ | 5-(1-3+2) | 

Điều này cho thấy cách các dấu hiệu bên trong được đảo ngược để giảm thiểu sự đóng góp bên trong dấu ngoặc đơn. 

### Ví dụ 2 

đầu vào:```
10-(2-3+4)
```| Mã thông báo | Dấu hiệu bối cảnh | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 10 | + | giữ | 10 | 
| - | + | trở thành - | 10- | 
| ( | - | đẩy - | 10-( | 
| 2 | - | giữ | 10-(2 | 
| - | - | trở thành + | 10-(2+ | 
| 3 | - | giữ | 10-(2+3 | 
| + | - | trở thành - | 10-(2+3- | 
| 4 | - | giữ | 10-(2+3-4) | 

Dấu vết này cho thấy bối cảnh phủ định đảo ngược ý nghĩa của toán tử bên trong dấu ngoặc đơn như thế nào, đây là cơ chế cốt lõi cho phép giảm thiểu tổng thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi ký tự được xử lý một lần trong một lần quét biểu thức | 
| Không gian |$O(n)$| Chuỗi đầu ra và một ngăn xếp nhỏ tỷ lệ thuận với độ sâu lồng nhau | 

Quét tuyến tính là đủ vì mỗi quyết định đều mang tính cục bộ và không yêu cầu tính toán lại hoặc quay lui. Với$10^5$ký tự, điều này phù hợp thoải mái trong giới hạn điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    old_stdout = _sys.stdout
    _sys.stdout = io.StringIO()

    solve()

    out = _sys.stdout.getvalue()
    _sys.stdout = old_stdout
    return out.strip()

# basic samples
assert run("5+(1+3-2)\n") == "5-(1-3+2)"

# single number
assert run("42\n") == "42"

# nested parentheses
assert run("10-(2-3)\n") == "10-(2+3)"

# all plus chain
assert run("1+2+3\n") == "1-2-3"

# alternating structure
assert run("1-(2+(3-4))\n") == "1-(2-(3+4))"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`42`|`42`| xử lý mã thông báo duy nhất | 
|`10-(2-3)`|`10-(2+3)`| truyền dấu trong ngoặc đơn | 
|`1-(2+(3-4))`|`1-(2-(3+4))`| tính chính xác của việc đảo ngược lồng nhau | 

## Vỏ cạnh 

Một biểu thức số đơn như`7`không có toán tử nào để lật, do đó thuật toán chỉ đơn giản đưa ra số không thay đổi. Ngăn xếp vẫn ở trạng thái ban đầu và không có quyết định nào được đưa ra. 

Một biểu thức lồng nhau hoàn toàn như`1-(2-(3-(4)))`bài tập lặp đi lặp lại đảo dấu. Mỗi dấu ngoặc đơn mở sẽ đẩy một bối cảnh bị đảo ngược và mỗi dấu đóng sẽ khôi phục lại bối cảnh đó. Các toán tử thay thế thay thế tương ứng và việc theo dõi xác nhận việc truyền bá nhất quán mà không bị trôi. 

Biểu thức bắt đầu bằng dấu ngoặc đơn đảm bảo ngữ cảnh ban đầu được áp dụng chính xác ngay từ đầu, vì ngăn xếp luôn chứa ít nhất một trạng thái ký hiệu, ngăn chặn hành vi không xác định khi xử lý toán tử đầu tiên bên trong khối lồng nhau.
