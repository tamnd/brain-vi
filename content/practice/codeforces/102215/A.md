---
title: "CF 102215A - Phòng và lối đi"
description: "Ngục tối là một chuỗi thẳng gồm (n+1) phòng, vì vậy mỗi lối đi chỉ đơn giản là di chuyển chúng ta sang bên phải một vị trí. Đoạn (i) được đại diện bởi (ai). Giá trị tuyệt đối của nó là màu của thẻ mà nó sử dụng."
date: "2026-08-25T03:48:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "A"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 3029
verified: true
draft: false
---

[CF 102215A - Phòng và lối đi](https://codeforces.com/problemset/problem/102215/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ngục tối là một chuỗi thẳng gồm (n+1) phòng, vì vậy mỗi lối đi chỉ đơn giản là di chuyển chúng ta sang bên phải một vị trí. Đoạn (i) được biểu thị bằng (a_i). Giá trị tuyệt đối của nó là màu của thẻ mà nó sử dụng. Giá trị dương có nghĩa là đoạn kiểm tra đã vượt qua, trong khi giá trị âm có nghĩa là đoạn văn luôn có thể bị vượt qua nhưng màu đó sẽ vô hiệu vĩnh viễn. 

Chúng tôi cần câu trả lời cho mọi phòng bắt đầu từ (0) đến (n-1). Câu trả lời là số đoạn chúng ta có thể vượt qua thành công trước đoạn đầu tiên từ chối chúng ta. Vì mỗi lối đi thành công đều đi vào một phòng mới nên đây cũng chính là số lượng phòng mới đạt được. Giới hạn đầu vào (n) đến (500000), như được nêu trên trang vấn đề chính thức. 

Sự tương tác chính là giữa lần xuất hiện tiêu cực và lần xuất hiện tích cực sau đó của cùng một màu. Nếu chúng ta vượt qua một đoạn phủ định (-c), thì đoạn văn (c) sẽ không hợp lệ. Bất kỳ điều gì muộn hơn (+c) đều trở thành không thể. Bản thân một đoạn tiêu cực không bao giờ ngăn cản được chúng ta. 

Ví dụ,```
3
1 -1 1
```có câu trả lời```
2 2 1
```Bắt đầu từ phòng (0), chúng ta băng qua (+1), sau đó (-1), và cuối cùng (+1) bị chặn nên hai đoạn đi qua. Bắt đầu từ phòng (1), chúng ta băng qua (-1), vô hiệu hóa màu (1) và ngay lập tức bị chặn bởi lối đi cuối cùng nên chỉ băng qua một lối đi. Một giải pháp bất cẩn chỉ kiểm tra xem màu tương tự có xuất hiện ở đâu đó sau đó mà không tôn trọng vị trí bắt đầu hay không, có thể áp dụng sai đoạn phủ định đầu tiên cho các phần bắt đầu xảy ra sau nó. 

Một trường hợp cạnh khác là một đoạn âm không có sự xuất hiện dương sau đó của cùng một màu.```
3
-1 -2 -3
```Câu trả lời là```
3 2 1
```Mọi đoạn tiêu cực luôn có thể được vượt qua và không có đoạn nào không hợp lệ được kiểm tra sau đó. Việc coi mọi đoạn văn phủ định như một điểm dừng có thể sẽ tạo ra những câu trả lời nhỏ hơn một cách không chính xác. 

Những đoạn tiêu cực lặp đi lặp lại cũng quan trọng. Coi như```
3
1 -1 -1
```Câu trả lời là```
3 2 1
```Bắt đầu từ phòng (0), đoạn đầu tiên là dương và thành công, còn cả hai đoạn sau đều âm nên cả ba đoạn đều bị gạch chéo. Một phương pháp giả định mọi sự vô hiệu cuối cùng phải gây ra lỗi sẽ dừng lại ở đoạn thứ hai một cách không chính xác. 

Mô phỏng lực lượng vũ phu sẽ dễ thực hiện, nhưng giới hạn (n=500000) loại trừ nó. Với thuật toán (O(n^2)), trường hợp xấu nhất yêu cầu khoảng (n(n+1)/2), tức là khoảng (1,25\times10^{11}) kiểm tra đoạn. Điều đó vượt xa những gì giới hạn cuộc thi hai giây cho phép. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là bắt đầu từ mỗi phòng và mô phỏng việc đi bộ một cách độc lập. Chúng tôi duy trì những màu nào hiện hợp lệ, di chuyển từ trái sang phải, vô hiệu hóa một màu bất cứ khi nào chúng tôi gặp một đoạn phủ định và dừng lại khi một đoạn tích cực yêu cầu một màu không hợp lệ. Điều này đúng vì nó tái tạo chính xác các quy tắc của ngục tối. 

Vấn đề là các vị trí bắt đầu liên tiếp liên tục kiểm tra gần như cùng một hậu tố. Ví dụ: nếu tất cả các đoạn đều tích cực thì phần bắt đầu tại phòng (0) sẽ kiểm tra tất cả (n) đoạn, phần bắt đầu tại phòng (1) sẽ kiểm tra (n-1), v.v. Tổng công là (n(n+1)/2), cho (O(n^2)) thời gian. 

Nhận xét hữu ích là cách duy nhất mà một đoạn văn có thể ngăn cản chúng ta là một đoạn văn tích cực (+c) đã được đặt trước, kể từ điểm xuất phát của chúng ta, bởi một đoạn văn phủ định (-c). Thay vì mô phỏng tập hợp các thẻ hợp lệ hiện tại cho mỗi lần bắt đầu, chúng ta có thể xử lý ngược mảng. 

Khi quét từ phải sang trái, với mỗi màu, chúng ta có thể nhớ đoạn tích cực gần nhất của màu đó ở bên phải. Khi chúng ta gặp một đoạn văn phủ định (-c), đoạn văn tích cực được ghi nhớ đó chính xác là đoạn văn đầu tiên trong tương lai không thể thực hiện được vì đoạn văn phủ định này. Do đó, đoạn phủ định này tạo ra một giới hạn trên về khoảng cách mà điểm xuất phát tại hoặc trước khi nó có thể đi được. 

Có thể có một số giới hạn như vậy từ các màu khác nhau. Chúng tôi chỉ quan tâm đến điểm dừng sớm nhất, vì vậy tất cả chúng có thể được biểu diễn bằng một biến chứa chỉ số đoạn cuối được phép nhỏ nhất. Quét ngược cho phép chúng tôi cập nhật biến đó một lần và sử dụng lại biến đó cho mọi vị trí bắt đầu trước đó. 

Đây là sự tái diễn ngược tương tự đằng sau giải pháp tiêu chuẩn cho vấn đề này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Ngược DP | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sử dụng chỉ mục dựa trên 1 cho các đoạn văn. Xác định (dp[i]) là số đoạn có thể vượt qua khi bắt đầu ngay trước đoạn (i). Câu trả lời bắt buộc cho (các) phòng khi đó là (dp[s+1]). 
2. Quét các đoạn văn từ (n) đến (1). Duy trì`next_pos[c]`, đoạn màu dương gần nhất (c) đã được nhìn thấy trong quá trình quét ngược. Nếu không có đoạn văn đó tồn tại thì giá trị của nó bằng 0. 
3. Đồng thời duy trì`limit`, chỉ số thông qua nhỏ nhất vẫn có thể được vượt qua trong số tất cả các hạn chế được phát hiện cho đến nay. Ban đầu không có hạn chế nào nên hãy đặt`limit = n + 1`. 
4. Khi (a_i>0), đoạn văn (i) luôn có thể vượt qua được khi chúng ta đến nó, bởi vì mọi hạn chế có khả năng vô hiệu hóa đường chuyền của nó phải ở bên trái của nó so với lần quét hiện tại. Sau khi vượt qua nó, hành trình còn lại chính xác là tình huống được biểu thị bằng (dp[i+1]). Như vậy thiết lập 
[ 
dp[i]=dp[i+1]+1. 
] 
Sau đó lưu trữ`next_pos[a_i] = i`, bởi vì đây hiện là đoạn tích cực gần nhất của màu đó ở bên phải của mọi vị trí trước đó. 
5. Khi (a_i<0), bản thân đoạn văn (i) không bao giờ cản trở chúng ta. Nó làm mất hiệu lực màu (-a_i). Nếu không có sự chuyển tiếp tích cực của màu đó sang bên phải của nó thì sự vô hiệu này không bao giờ thành vấn đề, vì vậy một lần nữa 
[ 
dp[i]=dp[i+1]+1. 
] 
6. Nếu một đoạn văn khẳng định (p=\text{next_pos[-a_i]) tồn tại, việc vượt qua đoạn văn (i) sẽ khiến đoạn văn (p) không thể thực hiện được. Do đó, bắt đầu từ hoặc trước đoạn (i), chúng ta không thể vượt qua đoạn (p-1). cập nhật 
[ 
\text{limit}=\min(\text{limit},p-1). 
] 
Điểm bắt đầu hiện tại có thể đi qua các đoạn (i,i+1,\ldots,\text{limit}), vì vậy 
[ 
dp[i]=\text{limit}-i+1. 
] 
các`limit`cần phải thay đổi vì một đoạn phủ định trước đó có thể đã áp đặt một điểm dừng thậm chí còn nhỏ hơn. 
7. Sau khi xử lý từng đoạn, xuất ra (dp[1],dp[2],\ldots,dp[n]). Chúng tương ứng trực tiếp với số lần bắt đầu (0,1,\ldots,n-1). 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý hậu tố (i,\ldots,n),`next_pos[c]`là đoạn tích cực đầu tiên của màu (c) trong hậu tố đó, trong khi`limit`là đoạn văn sớm nhất bị cấm bởi một số đoạn phủ định đã được xử lý ở hậu tố. Một đoạn khẳng định có thể được gạch bỏ khi nó là đoạn đầu tiên hiện tại, vì vậy câu trả lời của nó là một cộng với câu trả lời của hậu tố còn lại. Một đoạn tiêu cực luôn có thể được vượt qua, nhưng nếu màu của nó có sự xuất hiện tích cực trong tương lai, thì đoạn tích cực đó sẽ bị cấm, tạo ra chính xác giới hạn mới`p - 1`. Lấy mức tối thiểu sẽ duy trì hạn chế sớm nhất đối với mọi màu sắc. Do đó, mỗi (dp[i]) được tính toán chính xác là số đoạn văn liên tiếp tối đa có thể được vượt qua từ vị trí đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))

    # next_pos[c] = nearest positive passage of color c
    # to the right of the current position.
    next_pos = [0] * (n + 1)

    # dp[i] = number of passages that can be crossed
    # starting immediately before passage i.
    dp = [0] * (n + 2)

    # No restriction exists initially.
    limit = n + 1

    for i in range(n, 0, -1):
        x = a[i]

        if x > 0:
            # A positive passage can always be crossed at this point.
            dp[i] = dp[i + 1] + 1

            # It becomes the closest positive occurrence of this color
            # for all positions to its left.
            next_pos[x] = i

        else:
            color = -x
            p = next_pos[color]

            if p == 0:
                # No future positive passage uses this color,
                # so invalidating it has no effect.
                dp[i] = dp[i + 1] + 1
            else:
                # Passage p will be blocked after crossing i.
                limit = min(limit, p - 1)

                # We can cross from i through limit.
                dp[i] = limit - i + 1

    print(*dp[1:n + 1])

if __name__ == "__main__":
    solve()
```Mảng đầu vào được lưu trữ với số 0 giả ở chỉ mục (0), cho phép các số đoạn khớp trực tiếp với các chỉ số dựa trên toán học 1 của chúng. Điều này loại bỏ một số chuyển đổi có thể xảy ra từng cái một.`next_pos`có một mục nhập cho mọi màu vượt qua có thể. Vì màu sắc được đảm bảo tối đa là (n) nên danh sách đơn giản sẽ nhanh hơn và đơn giản hơn từ điển. 

Vòng lặp ngược lại tính toán`dp[i]`trước khi di chuyển xa hơn về bên trái. Đối với một giá trị dương, việc gán cho`next_pos`phải xảy ra sau khi tính toán`dp[i]`, bởi vì đoạn dương hiện tại không phải là đoạn ở bên phải của chính nó. Đối với giá trị âm, việc tra cứu diễn ra trước bất kỳ cập nhật nào vì lần xuất hiện dương liên quan phải được xử lý. 

biểu thức`limit - i + 1`đếm các đoạn văn một cách toàn diện. Nếu như`limit == i`, chính xác một đoạn văn có thể được vượt qua. Nếu như`limit == n`, tất cả các đoạn từ (i) đến (n) đều có thể được gạch chéo. Việc khởi tạo`limit = n + 1`thể hiện sự vắng mặt của bất kỳ hạn chế nào. 

Số nguyên Python không tràn cho các giá trị này. Việc triển khai chỉ thực hiện một lượng công việc không đổi trên mỗi đoạn, đó là lý do chính khiến nó có thể xử lý (n=500000). 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
6
1 -1 -1 1 -1 1
```quá trình quét ngược hoạt động như sau. 

| (i) | (a_i) |`next_pos[abs(a_i)]`trước |`limit`trước |`dp[i]`|`limit`sau | 
| --- | --- | --- | --- | --- | --- | 
| 6 | 1 | 0 | 7 | 1 | 7 | 
| 5 | -1 | 6 | 7 | 1 | 5 | 
| 4 | 1 | 6 | 5 | 2 | 5 | 
| 3 | -1 | 4 | 5 | 1 | 3 | 
| 2 | -1 | 4 | 3 | 2 | 3 | 
| 1 | 1 | 4 | 3 | 3 | 3 | 

Ở đoạn 5, số âm ( -1 ) làm cho đoạn 6, tức là (+1), không thể, vì vậy`limit`trở thành (5). Sau này khi chúng ta gặp đoạn 3, một phủ định khác ( -1 ) coi đoạn 4 là tương lai gần nhất (+1), tạo ra giới hạn chặt chẽ hơn (3). Đoạn 2 sau đó có thể vượt qua đoạn 2 và 3, đưa ra đáp án (2). Các câu trả lời cuối cùng là (3,2,1,2,1,1), phù hợp với mẫu. 

Đối với mẫu 2,```
7
2 -1 -2 -3 1 3 2
```quét ngược là: 

| (i) | (a_i) | Tương lai có liên quan tích cực |`limit`sau |`dp[i]`| 
| --- | --- | --- | --- | --- | 
| 7 | 2 | không có trước khi xử lý | 8 | 1 | 
| 6 | 3 | không có trước khi xử lý | 8 | 2 | 
| 5 | 1 | không có trước khi xử lý | 8 | 3 | 
| 4 | -3 | 6 | 5 | 2 | 
| 3 | -2 | 7 | 5 | 3 | 
| 2 | -1 | 5 | 4 | 3 | 
| 1 | 2 | 7 | 4 | 4 | 

Ở đoạn 4, màu (3) không hợp lệ và đoạn 6 là tương lai đầu tiên (+3), vì vậy đoạn 5 là đoạn xa nhất có thể. Đoạn 3 tạo ra hạn chế ở đoạn 7, nhưng giới hạn hiện tại của (5) đã nhỏ hơn. Đoạn 2 vô hiệu hóa màu (1), khiến đoạn 5 không thể thực hiện được và thắt chặt giới hạn thành (4). Các câu trả lời thu được là (4,3,3,2,3,2,1), một lần nữa khớp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi đoạn được xử lý chính xác một lần, với các phép toán mảng thời gian không đổi. | 
| Không gian | (O(n)) | Mỗi mảng đầu vào, mảng lập trình động và mảng dương gần nhất trên mỗi màu đều chứa các phần tử (O(n)). | 

Với (n\le500000), quá trình quét (O(n)) chỉ thực hiện vài triệu thao tác nguyên thủy, trong khi mô phỏng (O(n^2)) sẽ yêu cầu khoảng (1,25\times10^{11}) kiểm tra đoạn trong trường hợp xấu nhất. Giải pháp tuyến tính phù hợp thoải mái với giới hạn hai giây và 256 MB đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = [0] + list(map(int, input().split()))

    next_pos = [0] * (n + 1)
    dp = [0] * (n + 2)
    limit = n + 1

    for i in range(n, 0, -1):
        x = a[i]

        if x > 0:
            dp[i] = dp[i + 1] + 1
            next_pos[x] = i
        else:
            color = -x
            p = next_pos[color]

            if p == 0:
                dp[i] = dp[i + 1] + 1
            else:
                limit = min(limit, p - 1)
                dp[i] = limit - i + 1

    print(*dp[1:n + 1])

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("""6
1 -1 -1 1 -1 1
""") == "3 2 1 2 1 1", "sample 1"

assert run("""7
2 -1 -2 -3 1 3 2
""") == "4 3 3 2 3 2 1", "sample 2"

# Minimum size, positive passage.
assert run("""1
1
""") == "1", "minimum positive"

# Minimum size, negative passage.
assert run("""1
-1
""") == "1", "minimum negative"

# All passages have the same color and are negative.
# Nothing can block because there is no positive check.
assert run("""4
-1 -1 -1 -1
""") == "4 3 2 1", "all negative same color"

# Boundary case: a negative passage immediately invalidates
# the color checked by the next passage.
assert run("""3
2 -2 2
""") == "2 1 1", "immediate invalidation"

# A negative color may have no future positive occurrence.
assert run("""3
-1 -2 1
""") == "2 2 1", "unused invalidated color"

# Maximum-size input, all positive and therefore no passage can fail.
n = 500000
inp = str(n) + "\n" + ("1 " * n).strip() + "\n"
expected = " ".join(map(str, range(n, 0, -1)))
assert run(inp) == expected, "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Kích thước tối thiểu và xử lý lối đi tích cực | 
|`1 / -1`|`1`| Kích thước tối thiểu và một đoạn phủ định không thể chặn | 
|`4 / -1 -1 -1 -1`|`4 3 2 1`| Tất cả các giá trị có cùng màu, không có dấu tích dương | 
|`3 / 2 -2 2`|`2 1 1`| Ranh giới chính xác nơi việc vô hiệu ảnh hưởng đến đoạn văn ngay sau đó | 
|`3 / -1 -2 1`|`2 2 1`| Một màu tiêu cực không có sự xuất hiện tích cực trong tương lai | 
| (n=500000), tất cả`1`|`500000 499999 ... 1`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Đối với trường hợp vô hiệu ngay lập tức,```
3
2 -2 2
```quét ngược trước tiên sẽ thấy (+2) cuối cùng, vì vậy`next_pos[2] = 3`. Ở đoạn 2, giá trị là (-2), do đó việc vượt qua nó khiến đoạn 3 không thể thực hiện được. Giới hạn trở thành (3-1=2) và (dp[2]=1). Khi đoạn 1 được xử lý, nó mang giá trị dương và có thể được gạch chéo, do đó (dp[1]=dp[2]+1=2). Đầu ra là`2 1 1`. các`p - 1`tính toán là thứ ngăn cản việc tính chính lối đi bị chặn. 

Đối với màu âm không có sự xuất hiện tích cực trong tương lai,```
3
-1 -2 1
```quét ngược nhìn thấy (+1) ở đoạn 3, nhưng nó không bao giờ nhìn thấy dương (+2). Do đó đoạn 2 không có hạn chế và cho kết quả (dp[2]=dp[3]+1=2). Đoạn 1 có tương lai (+1), vì vậy nó đặt giới hạn thành (2), cho (dp[1]=2). Kết quả là`2 2 1`. Điều này chứng tỏ tại sao chỉ những đoạn phủ định có màu được kiểm tra sau mới cần ảnh hưởng đến`limit`. 

Đối với sự vô hiệu lặp đi lặp lại,```
3
1 -1 -1
```quá trình quét ngược xử lý cả hai đoạn âm trước khi đến đoạn dương. Ở đoạn 2, tương lai (+1) nằm ở đoạn 1, không nằm ở bên phải của nó, nên thực tế không có tương lai dương (+1) theo quan điểm của đoạn 2. Điều tương tự cũng đúng với đoạn 3. Do đó không có hạn chế nào được tạo ra và phép truy toán ngược cho kết quả`3 2 1`. Đây chính xác là hành vi của bước đi về phía trước: cả hai lối đi tiêu cực đều có thể được vượt qua và không có sự kiểm tra tích cực nào sau đó. 

Đối với đầu vào tối thiểu,```
1
-1
```đoạn văn duy nhất là phủ định, vì vậy nó luôn có thể vượt qua được. Quá trình quét ngược không tìm thấy đoạn tích cực nào trong tương lai, tính toán (dp[1]=dp[2]+1=1) và xuất ra`1`. Trọng điểm (dp[n+1]=0) làm cho trường hợp ranh giới này hoạt động mà không cần nhánh đặc biệt. 

Đối với trường hợp kích thước tối đa, mọi đoạn văn có thể được đặt thành (+1). Không có thẻ nào bị vô hiệu, do đó, bắt đầu từ (các) phòng sẽ đến phòng (n) sau khi đi qua chính xác (n-s) đoạn. Thuật toán chỉ đơn giản áp dụng phép truy hồi dương (n) lần, tạo ra (500000,499999,\ldots,1). Điều này thực hiện kích thước đầu vào đầy đủ trong khi vẫn giữ cho hoạt động của thuật toán hoàn toàn tuyến tính.
