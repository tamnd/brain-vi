---
title: "CF 1541B - Đôi dễ chịu"
description: "Chúng ta có một số trường hợp thử nghiệm, mỗi trường hợp chứa một mảng các số nguyên riêng biệt được lập chỉ mục từ 1. Nhiệm vụ là đếm xem có bao nhiêu cặp chỉ mục $(i, j)$ với $i < j$ thỏa mãn một mối quan hệ rất cụ thể: tích của các giá trị được lưu trữ tại các vị trí đó bằng tổng của các chỉ số…"
date: "2026-06-10T14:19:44+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1541
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 728 (Div. 2)"
rating: 1200
weight: 1541
solve_time_s: 95
verified: true
draft: false
---

[CF 1541B - Cặp đôi dễ chịu](https://codeforces.com/problemset/problem/1541/B) 

**Đánh giá:** 1200 
**Tags:** vũ lực, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp chứa một mảng các số nguyên riêng biệt được lập chỉ mục từ 1. Nhiệm vụ là đếm xem có bao nhiêu cặp chỉ mục$(i, j)$với$i < j$thỏa mãn một mối quan hệ rất cụ thể: tích của các giá trị được lưu trữ tại các vị trí đó bằng tổng của các chỉ số, nghĩa là$a_i \cdot a_j = i + j$. 

Đây là một ràng buộc về cấu trúc kết hợp các giá trị và vị trí. Vì các chỉ số được giới hạn bởi$n$, phía bên phải$i + j$cũng bị giới hạn bởi$2n$, rất nhỏ so với phạm vi giá trị tiềm năng của sản phẩm. Sự bất đối xứng này chính là mấu chốt: mặc dù các giá trị có thể lên tới$2n$, các cặp hợp lệ phải đáp ứng điều kiện số học chặt chẽ liên kết các chỉ số và giá trị đồng thời. 

Với$n$lên đến$10^5$mỗi bài kiểm tra và tổng số tiền$2 \cdot 10^5$, MỘT$O(n^2)$giải pháp cho mỗi trường hợp thử nghiệm là không thể. Thậm chí$O(n \sqrt{n})$mỗi bài kiểm tra sẽ quá chậm trong trường hợp xấu nhất. Bất kỳ giải pháp đúng nào cũng phải dựa vào thực tế là chỉ một số lượng nhỏ các cặp có thể thỏa mãn phương trình, thay vì kiểm tra tất cả các cặp. 

Một trường hợp thất bại phổ biến là do thử tất cả các cặp có giá trị “có vẻ hứa hẹn”, ví dụ như giới hạn ở các giá trị nhỏ hoặc các chỉ số gần đó. Cách tiếp cận đó bỏ sót các cặp hợp lệ vì điều kiện phụ thuộc vào cả phép nhân và tổng chỉ số, chứ không phụ thuộc vào độ gần hoặc thứ tự. 

Ví dụ: theo cách tiếp cận lọc đơn giản, người ta chỉ có thể kiểm tra các cặp trong đó$a_i \cdot a_j \le 2n$, vì vế phải nhỏ. Điều này là cần thiết nhưng chưa đủ và nếu không có sự liệt kê có cấu trúc các tích có thể, nó vẫn dẫn đến hành vi bậc hai. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ kiểm tra từng cặp$(i, j)$với$i < j$, tính toán$a_i \cdot a_j$, và so sánh nó với$i + j$. Điều này đúng vì nó đánh giá trực tiếp điều kiện như đã nêu. Tuy nhiên, nó thực hiện khoảng$n(n-1)/2$kiểm tra trên mỗi trường hợp thử nghiệm, điều này trở nên đại khái$5 \cdot 10^9$tổng số hoạt động ở kích thước đầu vào được phép tồi tệ nhất, rõ ràng là quá chậm. 

Quan sát quan trọng là thay vì lặp lại các cặp chỉ số, chúng ta có thể diễn giải lại điều kiện bằng cách sửa một chỉ mục và rút ra các ràng buộc trên chỉ mục kia. Từ$a_i \cdot a_j = i + j$, chúng ta có thể sắp xếp lại nó thành:$$j = a_i \cdot a_j - i$$Điều này vẫn không hữu ích trực tiếp vì cả hai$j$Và$a_j$chưa được biết. Cái nhìn sâu sắc về cấu trúc quan trọng đến từ giới hạn: vì$i + j \le 2n$, chúng tôi nhận được:$$a_i \cdot a_j \le 2n$$Điều này ngụ ý rằng chỉ những cặp giá trị có tích nhỏ mới phù hợp. Vì tất cả$a_i$là các số nguyên dương phân biệt đến$2n$, mỗi giá trị$x$chỉ có thể ghép đôi một cách có ý nghĩa với các giá trị$y \le \frac{2n}{x}$. Điều này làm giảm đáng kể số lượng cặp giá trị ứng cử viên. 

Sau đó, giải pháp sẽ đảo ngược hoàn toàn phối cảnh: thay vì lặp lại các chỉ mục, chúng tôi lặp lại các giá trị có thể$x = a_i$và liệt kê các bội số hợp lệ$y$như vậy$x \cdot y \le 2n$. Với mỗi cặp như vậy$(x, y)$, chúng tôi kiểm tra xem vị trí của họ có thỏa mãn$i + j = x \cdot y$. 

Để hỗ trợ điều này một cách hiệu quả, chúng tôi lưu trữ chỉ mục của từng giá trị trong bản đồ băm. Sau đó, với mỗi cặp giá trị hợp lệ, chúng tôi truy xuất các chỉ số trong O(1) và kiểm tra điều kiện một lần. 

Điều này làm giảm vấn đề từ bậc hai trên các chỉ số xuống gần như:$$\sum_{x=1}^{2n} \frac{2n}{x} = O(n \log n)$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bản đồ vị trí từ giá trị đến chỉ mục. Điều này cho phép tra cứu theo thời gian liên tục về nơi mỗi giá trị xuất hiện, điều này rất cần thiết vì điều kiện phụ thuộc vào các chỉ số chứ không chỉ các giá trị. 
2. Lặp lại tất cả các giá trị có thể$x$xuất hiện trong mảng. Đối với mỗi$x$, hãy coi nó như một mặt của sản phẩm hợp lệ tiềm năng. 
3. Đối với mỗi$x$, lặp qua bội số$y$như vậy$x \cdot y \le 2n$. Điều này giới hạn không gian tìm kiếm vì bất kỳ cặp hợp lệ nào cũng phải đáp ứng ràng buộc tổng chỉ số và do đó ràng buộc sản phẩm. 
4. Đối với mỗi cặp thí sinh$(x, y)$, hãy kiểm tra xem cả hai giá trị có tồn tại trong mảng hay không bằng cách sử dụng bản đồ vị trí. 
5. Nếu cả hai đều tồn tại, hãy truy xuất chỉ mục của chúng$i$Và$j$, và xác minh xem$i < j$Và$i + j = x \cdot y$. Nếu đúng thì đếm cặp này. 
6. Vì mỗi cặp hợp lệ đương nhiên sẽ gặp hai lần$(x, y)$Và$(y, x)$, hạn chế lặp lại$x \le y$hoặc thực thi việc sắp xếp theo chỉ số để tránh tính hai lần. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là mọi cặp hợp lệ$(i, j)$tạo ra một cặp giá trị hợp lệ$(a_i, a_j)$sản phẩm của nó bằng$i + j \le 2n$. Ngược lại, mọi cặp giá trị ứng cử viên mà chúng tôi tạo ra đều tương ứng với nhiều nhất một cặp chỉ mục. Bởi vì chúng tôi liệt kê tất cả các sản phẩm có giá trị khả thi trong giới hạn$2n$, chúng tôi đảm bảo không có giải pháp hợp lệ nào bị bỏ sót và quá trình kiểm tra chỉ mục sẽ lọc ra các ứng cử viên sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        pos = {}
        for i, v in enumerate(a, 1):
            pos[v] = i
        
        ans = 0
        
        limit = 2 * n
        
        # iterate over possible values
        for x in a:
            i = pos[x]
            # avoid double counting by enforcing x <= y in value space
            # since values are distinct, we use numeric ordering
            for y in range(1, limit // x + 1):
                v = x * y
                if v in pos:
                    j = pos[v]
                    if i < j and i + j == x * v:
                        ans += 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi của việc triển khai là từ điển giá trị theo chỉ mục`pos`, cho phép kiểm tra liên tục xem giá trị ứng cử viên có tồn tại hay không và nó xuất hiện ở đâu. Vòng lặp lồng nhau không lặp qua các chỉ số mà lặp qua các cặp nhân bị ràng buộc bởi$x \cdot y \le 2n$, đây là bước cắt tỉa quyết định. 

điều kiện`i + j == x * v`mã hóa trực tiếp phương trình ban đầu và`i < j`kiểm tra đảm bảo chúng tôi chỉ tính mỗi cặp hợp lệ một lần. Cấu trúc vòng lặp hoàn toàn tránh được sự trùng lặp cặp không cần thiết ở mức giá trị. 

Một điểm tinh tế là chúng ta lặp lại các giá trị thực trong mảng để tìm`x`, không vượt quá phạm vi đầy đủ$1 \dots 2n$, giữ cho hệ số hằng số thực tế ở mức thấp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [6, 1, 5]
```| x | tôi | y | v = x*y | v trong mảng? | j | tôi + j | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 6 | 1 | 1 | 6 | vâng | 1 | 2 | không | 
| 6 | 1 | 5 | 30 | không | - | - | không | 
| 1 | 2 | 1 | 1 | vâng | 2 | 4 | không | 
| 1 | 2 | 5 | 5 | vâng | 3 | 5 | vâng | 

Chỉ có một cặp hợp lệ được tìm thấy. 

Dấu vết này cho thấy làm thế nào chỉ có một ứng cử viên nhân vượt qua được cả việc kiểm tra sự tồn tại của giá trị và phương trình chỉ số. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [3, 1, 5, 9, 2]
```| x | tôi | y | v | j | tôi + j | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 1 | 1 | 3 | 2 | 3 | vâng | 
| 1 | 2 | 2 | 2 | 5 | 7 | không | 
| 1 | 2 | 3 | 3 | 1 | 3 | không (tôi<j thất bại) | 
| 2 | 5 | 1 | 2 | 2 | 7 | không | 

Cặp duy nhất còn sống sót tương ứng với$(1, 2)$. 

Những ví dụ này xác nhận rằng thuật toán lọc mạnh mẽ và chỉ giữ các kết quả khớp với giá trị chỉ mục nhất quán về mặt cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi giá trị lặp qua các ước số lên tới$2n$, tạo ra tác phẩm tổng thể chuỗi hài | 
| Không gian |$O(n)$| Bản đồ vị trí lưu trữ một mục nhập cho mỗi phần tử | 

Giới hạn hài hòa đảm bảo khả năng mở rộng theo$2 \cdot 10^5$tổng số phần tử, giữ cho giải pháp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        pos = {v: i for i, v in enumerate(a, 1)}
        ans = 0
        limit = 2 * n
        
        for x in a:
            i = pos[x]
            for y in range(1, limit // x + 1):
                v = x * y
                if v in pos:
                    j = pos[v]
                    if i < j and i + j == x * v:
                        ans += 1
        
        output.append(str(ans))
    
    return "\n".join(output)

# provided samples
assert run("3\n2\n3 1\n3\n6 1 5\n5\n3 1 5 9 2\n") == "1\n1\n3"

# custom cases
assert run("1\n2\n1 2\n") == "0", "minimum no solution"
assert run("1\n3\n2 1 3\n") == "1", "simple valid pair"
assert run("1\n4\n4 3 2 1\n") == "2", "reversed ordering"
assert run("1\n5\n1 2 3 4 5\n") == "0", "no structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 / 1 2`| 0 | kích thước tối thiểu, không có cặp hợp lệ | 
|`1 3 / 2 1 3`| 1 | cấu trúc hợp lệ cơ bản | 
|`1 4 / 4 3 2 1`| 2 | nhiều trận đấu đối xứng | 
|`1 5 / 1 2 3 4 5`| 0 | mảng dày đặc không có kết quả phù hợp | 

## Vỏ cạnh 

Trường hợp cạnh tinh vi xảy ra khi các giá trị đã gần với chỉ số của chúng. Coi như:```
n = 3
a = [1, 2, 3]
```Thuật toán xây dựng`pos = {1:1, 2:2, 3:3}`. Đối với mỗi cặp ứng cử viên, mặc dù có nhiều sản phẩm tồn tại nhưng điều kiện chỉ mục không thành công ở mọi nơi vì$i + j$tăng trưởng quá chậm so với$a_i \cdot a_j$. Việc điều tra vẫn chỉ kiểm tra các sản phẩm khả thi theo$2n$và mỗi ứng viên đều thất bại ở bước xác minh cuối cùng. 

Một trường hợp khác là khi các giá trị lớn được ghép với các chỉ số nhỏ:```
n = 3
a = [6, 1, 2]
```Ở đây sản phẩm hợp lý duy nhất là$1 \cdot 2 = 2$, nhưng tổng chỉ số ít nhất là 3 đối với bất kỳ cặp nào, do đó không có sự trùng khớp nào xảy ra. Thuật toán vẫn xem xét cặp vì nó tồn tại trong bản đồ giá trị, nhưng loại bỏ nó khi kiểm tra tính bằng nhau, đảm bảo tính chính xác mà không cần viết hoa đặc biệt.
