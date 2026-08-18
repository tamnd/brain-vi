---
title: "CF 102249A - Nhảy vọt: Ch. 1"
description: "Chúng tôi có một hàng (N) miếng hoa huệ. Tấm đầu tiên chứa Alpha Frog, mọi tấm còn lại chứa Beta Frog hoặc không có gì. Ếch Alpha chỉ có thể di chuyển sang bên phải."
date: "2026-08-17T21:59:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102249
codeforces_index: "A"
codeforces_contest_name: "2019 Facebook Hacker Cup, Qualification Round"
rating: 0
weight: 102249
solve_time_s: 89
verified: true
draft: false
---

[CF 102249A - Nhảy vọt: Ch. 1](https://codeforces.com/problemset/problem/102249/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng (N) miếng hoa huệ. Tấm đầu tiên chứa Alpha Frog, mọi tấm còn lại chứa Beta Frog hoặc không có gì. Ếch Alpha chỉ có thể di chuyển sang bên phải. Để thực hiện một nước đi, nó phải nhảy qua ít nhất một con ếch Beta liên tiếp và đáp xuống ô trống đầu tiên sau những con ếch Beta đó. Ếch Beta có thể di chuyển độc lập một vị trí sang trái hoặc phải bất cứ khi nào bảng đích trống. 

Câu hỏi đặt ra là liệu một số chuỗi di chuyển của Ếch Beta và các bước nhảy của Ếch Alpha cuối cùng có thể đưa Ếch Alpha lên tấm đệm cuối cùng hay không. 

Đầu vào chứa tối đa 500 trường hợp thử nghiệm và mỗi hàng có độ dài tối đa là 5000. Giải pháp quét mỗi hàng một lần hoặc một số lần không đổi nhỏ là đủ nhanh. Giải pháp (O(N^2)) cũng có thể được quản lý cho một trường hợp thử nghiệm duy nhất, nhưng với 500 trường hợp và độ dài gần 5000, giải pháp đó có thể yêu cầu hàng tỷ thao tác. Tìm kiếm theo cấp số nhân là hoàn toàn không khả thi, vì ngay cả một hàng vài chục miếng đệm cũng có thể có vô số cách sắp xếp ếch khả dĩ. 

Các trường hợp quan trọng rất dễ bị bỏ sót nếu chúng ta chỉ tập trung vào việc liệu Alpha Frog hiện có bước nhảy hợp pháp hay không. 

Vì`A.`, có một miếng đệm cuối cùng trống, nhưng không có Beta Frog để nhảy qua. Câu trả lời đúng là`N`. Một giải pháp bất cẩn xử lý một ô trống ở bên phải là đủ sẽ trả về không chính xác`Y`. 

Vì`AB`, có một con ếch Beta, nhưng không có bãi trống nào để ếch Alpha có thể hạ cánh. Câu trả lời đúng là`N`. Việc chỉ kiểm tra xem có ít nhất một Beta Frog tồn tại là sai. 

Vì`ABB`, có Ếch Beta nhưng không có miếng đệm trống nào cả, vì vậy Ếch Alpha không bao giờ có thể hạ cánh ở bất cứ đâu. Câu trả lời đúng là`N`. 

Vì`A.B`, có một Beta Frog và một miếng đệm trống. Ếch Beta có thể di chuyển sang trái để sản xuất`AB.`, sau đó Alpha Frog nhảy qua nó và đến tấm đệm cuối cùng. Câu trả lời đúng là`Y`. Một giải pháp chỉ kiểm tra xem nước đi đầu tiên có sẵn ngay lập tức hay không sẽ từ chối trường hợp này một cách không chính xác. 

Điểm khác biệt quan trọng là vị trí ban đầu của Ếch Beta không quan trọng bằng tổng số lượng của chúng. Ếch Beta có thể hợp tác bằng cách di chuyển qua các ô trống, vì vậy chúng ta có thể giải quyết vấn đề chỉ bằng cách đếm`B`Và`.`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ coi mọi cách sắp xếp có thể có của ếch đều là một trạng thái. Từ một trạng thái, chúng tôi có thể tạo ra mọi bước di chuyển của Ếch Beta hợp pháp và mọi bước nhảy của Ếch Alpha hợp pháp, sau đó chạy BFS hoặc DFS cho đến khi Ếch Alpha đến ô cuối cùng. 

Điều này đúng vì mọi chuỗi di chuyển hợp pháp đều tương ứng với một đường đi qua biểu đồ trạng thái này. Vấn đề là kích thước của biểu đồ đó. Nếu có (b) Ếch Beta, một trạng thái có thể được mô tả bằng cách chọn vị trí của Ếch Alpha và sau đó chọn (b) vị trí Beta trong số các miếng đệm (N-1) còn lại. Điều đó mang lại nhiều nhất 

[ 
N\binom{N-1}{b} 
] 

tiểu bang. Trên tất cả các giá trị có thể có của (b), đây là hàm mũ của (N). Việc tạo ra tối đa (O(N)) di chuyển từ mọi trạng thái sẽ tạo ra giới hạn trên trong trường hợp xấu nhất theo kiểu (O(N^2 2^N)). Với (N=5000), cách tiếp cận này không thực tế chút nào. 

Brute-force hoạt động vì nó theo dõi rõ ràng mọi sắp xếp có thể, nhưng đó chính xác là phần không cần thiết. Chúng tôi thực sự không quan tâm đến việc từng cá thể ếch Beta ở đâu. Khả năng di chuyển qua các miếng đệm trống của chúng có nghĩa là, miễn là có đủ tổng số Ếch Beta, chúng ta có thể mang một trong số chúng đến cạnh Ếch Alpha bất cứ khi nào Ếch Alpha cần nhảy. 

Gọi (b) là số lượng ếch Beta và (d) số lượng miếng đệm trống. Vì Alpha Frog chiếm một miếng đệm, 

[ 
N = 1+b+d. 
] 

Điều kiện cần đầu tiên là (d\geq 1). Ếch Alpha cuối cùng phải hạ cánh trên một tấm đệm trống, vì vậy nếu không có tấm đệm trống nào thì việc đến được tấm đệm cuối cùng là không thể. 

Điều kiện thứ hai là (b\geq d). Mỗi lần nhảy Alpha Frog phải vượt qua ít nhất một Beta Frog. Hãy coi những chỗ trống ở phần hàng vẫn ở phía trước Alpha Frog như những cơ hội hạ cánh trong tương lai. Để vượt qua một ô trống như vậy, chúng ta cần ít nhất một Beta Frog có thể được đặt ngay trước nó. Nếu có nhiều miếng trống hơn Beta Frogs, cuối cùng chúng ta sẽ hết Beta Frogs trước khi kết thúc. 

Ngược lại, nếu (1\leq d\leq b), ếch luôn có thể hợp tác. Khi ô tiếp theo đã bị Ếch Beta chiếm giữ, Ếch Alpha có thể nhảy qua các Ếch Beta liên tiếp và đáp xuống ô trống tiếp theo. Khi ô tiếp theo trống, Ếch Beta gần nhất ở xa bên phải có thể di chuyển sang trái qua các ô trống cho đến khi nó tiếp giáp với Ếch Alpha. Con ếch Alpha sau đó có thể nhảy qua nó. Mỗi bước nhảy như vậy tiêu tốn ít nhất một Beta Frog từ hậu tố còn lại đồng thời loại bỏ một bãi đáp trống khỏi hậu tố đó, do đó bất đẳng thức (b\geq d) được giữ nguyên. 

Như vậy toàn bộ bài toán quy về điều kiện đơn giản 

[ 
1\leq d\leq b. 
] 

Tương tự, vì (N=1+b+d), điều này cũng có thể được viết là 

[ 
b+1\leq N-1\leq 2b. 
] 

Đặc tính giống nhau có thể được thể hiện dưới cả hai dạng, nhưng việc đếm trực tiếp các dấu chấm làm cho lý luận trở nên rõ ràng hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2 2^N)) giới hạn trên | (O(N2^N)) | Quá chậm | 
| Đếm`B`Và`.`| (O(N)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lượng Ếch Beta,`b`, và số lượng miếng đệm trống,`d`. 
2. Nếu`d == 0`, trở lại`N`. Không có nơi nào để Ếch Alpha hạ cánh, ngay cả khi có nhiều Ếch Beta. 
3. Nếu`b >= d`, trở lại`Y`. Có đủ số lượng Ếch Beta để cung cấp ít nhất một lần nhảy cho mỗi vị trí hạ cánh trống mà Ếch Alpha phải vượt qua. 
4. Nếu không thì trả lại`N`. Nhiều miếng đệm trống hơn Ếch Beta có nghĩa là Ếch Alpha cuối cùng cần một bước nhảy mà không có Ếch Beta nào có sẵn. 

Lý do số lượng đủ là vì Ếch Beta có thể di chuyển qua các miếng đệm trống. Vị trí ban đầu chính xác của chúng không hạn chế chiến lược cuối cùng khi tổng số lượng Ếch Beta đủ lớn. 

### Tại sao nó hoạt động 

Điều bất biến là, ở phần hàng vẫn ở phía trước Alpha Frog, số lượng Ếch Beta có sẵn ít nhất bằng số miếng đệm trống vẫn cần được chuyển qua. Ban đầu đây chính xác là điều kiện (b\geq d). Bất cứ khi nào Ếch Alpha tiến bộ, nó sẽ đáp xuống một bãi đất trống và đã vượt qua ít nhất một Ếch Beta. So với hậu tố còn lại, cả vị trí hạ cánh trống bắt buộc và số Ếch Beta có sẵn đều giảm ít nhất một, do đó bất đẳng thức vẫn có giá trị. Nếu ô tiếp theo trống, Ếch Beta ở bên phải của nó có thể được di chuyển sang trái cho đến khi nó tiếp giáp với Ếch Alpha, thực hiện được bước nhảy cần thiết. Nếu ô tiếp theo đã bị Ếch Beta chiếm giữ, Ếch Alpha có thể nhảy ngay lập tức. Do đó (1\leq d\leq b) vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        s = input().strip()

        b = s.count('B')
        d = s.count('.')

        answer = 'Y' if d >= 1 and b >= d else 'N'
        out.append(f"Case #{case}: {answer}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào đọc một hàng cho mỗi trường hợp kiểm thử. Độ dài chuỗi không cần phải được lưu trữ riêng vì quyết định chỉ phụ thuộc vào số lượng`B`Và`.`.`b = s.count('B')`đếm mọi con ếch Beta, trong khi`d = s.count('.')`đếm từng ô trống. Alpha Frog không được tính là một miếng đệm trống, vì vậy`d`chính xác là số lượng bãi đáp có sẵn ban đầu. 

điều kiện`d >= 1`xử lý trường hợp mọi miếng đệm ngoại trừ miếng đầu tiên đều bị Beta Frog chiếm giữ. Một hàng như vậy có rất nhiều ếch nhưng không có vị trí có thể hạ cánh. 

điều kiện`b >= d`là quan sát trung tâm. Không có mô phỏng, đệ quy, xây dựng biểu đồ hoặc sửa đổi chuỗi. 

Cũng không có hoạt động lập chỉ mục nào liên quan đến phần đệm cuối cùng, do đó không có phép tính ranh giới dựa trên 0 hoặc dựa trên một đặc biệt nào bị sai. Số nguyên Python không bị giới hạn, nhưng dù sao thì số lượng ở đây tối đa là 5000, do đó việc tràn số nguyên là không liên quan. 

## Ví dụ đã hoạt động 

### Mẫu 1:`AB.`Ở đây có một con ếch Beta và một miếng đệm trống. Điều kiện được thỏa mãn vì (b=1) và (d=1). 

| Bước | Vị trí Alpha | Số lượng Beta có sẵn | Miếng đệm trống | Hành động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 1 | Pad tiếp theo chứa`B`| 
| Nhảy | 3 | 0 | 0 ở hậu tố còn lại | Alpha nhảy qua`B`| 
| Kết quả | 3 | 0 | 0 | Alpha đến phần cuối cùng | 

Ếch Alpha có thể nhảy trực tiếp qua Ếch Beta và đáp xuống ô trống cuối cùng, vì vậy câu trả lời là`Y`. 

Đây là cấu hình thành công không cần thiết nhỏ nhất. Nó cũng cho thấy tại sao cả hai nguồn lực đều được yêu cầu. Phải có một con ếch Beta để nhảy qua và một tấm đệm trống để đáp xuống. 

### Mẫu 2:`AB`Có một con ếch Beta và không có miếng trống nào. Do đó (b=1) và (d=0). 

| Bước | Vị trí Alpha | Số lượng beta | Miếng đệm trống | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 0 | Không có bãi đáp trống | 
| Kết quả | 1 | 1 | 0 | Trở lại`N`| 

Ếch Alpha không thể nhảy vì ô duy nhất bên phải của nó đã bị Ếch Beta chiếm giữ và không có ô trống nào sau nó. Câu trả lời là`N`. 

Dấu vết này thực hiện điều kiện biên`d == 0`. Chỉ số lượng Beta Frog thôi là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) cho mỗi trường hợp thử nghiệm | Đếm`B`Và`.`quét hàng một số lần không đổi | 
| Không gian | (O(N)) | Bản thân chuỗi đầu vào sử dụng bộ nhớ (O(N)); thuật toán sử dụng (O(1)) không gian bổ sung | 

Với (N\leq5000) và tối đa 500 trường hợp kiểm thử, tổng lượng đầu vào có thể được xử lý sẽ được xử lý dễ dàng bằng cách quét tuyến tính. Thuật toán chỉ thực hiện việc đếm ký tự và số lượng so sánh không đổi, do đó nó phù hợp thoải mái trong giới hạn dự định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for case in range(1, t + 1):
        s = input().strip()
        b = s.count('B')
        d = s.count('.')
        answer = 'Y' if d >= 1 and b >= d else 'N'
        out.append(f"Case #{case}: {answer}")

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solution()
    finally:
        sys.stdin = old_stdin

sample_input = """8
AB.
AB.
ABB
A.BB
A..BB..B
A.B..BBB.
AB.........
A.B..BBBB.BB
"""

sample_output = """Case #1: Y
Case #2: Y
Case #3: N
Case #4: Y
Case #5: N
Case #6: Y
Case #7: N
Case #8: Y"""

# Provided samples
assert run(sample_input) == sample_output, "provided samples"

# Minimum-size rows
assert run("""2
A.
AB
""") == """Case #1: N
Case #2: N""", "minimum-size cases"

# Exactly equal numbers of B and dots
assert run("""2
A.B
A.B.B
""") == """Case #1: Y
Case #2: Y""", "b equals d"

# One fewer B than dots
assert run("""2
A.B.
A..B
""") == """Case #1: N
Case #2: N""", "b is smaller than d"

# All remaining pads are dots
assert run("""1
A.....
""") == """Case #1: N""", "no Beta Frogs"

# All remaining pads are Beta Frogs
assert run("""1
ABBBB
""") == """Case #1: N""", "no empty pads"

# Maximum-size successful case:
# N = 5000, b = 2500, d = 2499, so b >= d >= 1.
s = "A" + "B" * 2500 + "." * 2499
assert len(s) == 5000
assert run("1\n" + s + "\n") == "Case #1: Y", "maximum-size successful case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A.`Và`AB`|`N`,`N`| Kích thước tối thiểu và ranh giới hai tài nguyên | 
|`A.B`,`A.B.B`|`Y`,`Y`| Ranh giới đẳng thức (b=d) | 
|`A.B.`,`A..B`|`N`,`N`| Thất bại khi (b<d) | 
|`A.....`|`N`| Không có con ếch Beta nào cả | 
|`ABBBB`|`N`| Không có bãi đáp trống | 
| Chiều dài 5000 với 2500`B`và 2499`.`|`Y`| Kích thước đầu vào tối đa và ranh giới thành công chặt chẽ | 

## Vỏ cạnh 

cho`A.`, ta có (b=0) và (d=1). Thuật toán kiểm tra`d >= 1`, điều đó đúng, nhưng`b >= d`là sai vì (0<1). Nó trở lại`N`. Ếch Alpha không thể di chuyển vì nó không có Ếch Beta để nhảy qua. 

Vì`AB`, ta có (b=1) và (d=0). Điều kiện đầu tiên thất bại ngay lập tức vì không có ô trống. Kết quả là`N`. Ếch Alpha không thể hạ cánh sau khi nhảy qua Ếch Beta. 

Vì`ABB`, ta có (b=2) và (d=0). Việc có nhiều Beta Frog hơn cũng không giúp ích gì vì mọi ô sau Alpha Frog đều đã bị chiếm dụng. Kết quả vẫn còn`N`. Điều này mắc phải sai lầm phổ biến là chỉ kiểm tra xem có ít nhất hai con ếch hay không. 

Vì`A.B`, ta có (b=1) và (d=1). Thuật toán trả về`Y`. Trình tự hợp lệ là di chuyển Beta Frog từ ô thứ ba sang ô thứ hai, tạo ra`AB.`, sau đó để Alpha Frog nhảy đến phần đệm cuối cùng. Cấu hình ban đầu không cần phải có bước nhảy Alpha hợp pháp ngay lập tức. 

Vì`A.B.`, ta có (b=1) và (d=2). Thuật toán trả về`N`. Có hai miếng đệm trống cuối cùng phải được xử lý, nhưng chỉ có một Beta Frog. Beta Frog không thể được sử dụng lại làm tài nguyên nhảy độc lập thứ hai. 

Để có một hàng thành công có kích thước tối đa với 5000 miếng đệm, 2500 con ếch Beta và 2499 miếng đệm trống, chúng ta có (b=2500) và (d=2499). Cả hai bất đẳng thức bắt buộc đều đúng, nên đáp án là`Y`. Kích thước lớn không làm thay đổi lý do vì thuật toán chỉ phụ thuộc vào hai số đếm. 

Ranh giới trung tâm là (b=d). Khi các số bằng nhau và có ít nhất một ô trống, câu trả lời là`Y`. Khi (b=d-1), câu trả lời chuyển sang`N`. Đây là ranh giới mà giải pháp dựa trên mô phỏng có thể che khuất, trong khi giải pháp đếm sẽ bộc lộ trực tiếp.
