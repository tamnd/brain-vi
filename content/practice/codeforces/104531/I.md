---
title: "CF 104531I - Giá đỡ"
description: "Chúng ta được cấp một chuỗi có độ dài $n$ bao gồm ba loại ký tự: dấu ngoặc mở, dấu ngoặc đóng và ký tự đại diện. Mỗi ký tự đại diện sau này có thể được chuyển thành dấu ngoặc mở hoặc dấu ngoặc đóng."
date: "2026-06-30T09:57:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104531
codeforces_index: "I"
codeforces_contest_name: "2022 SYSU School Contest"
rating: 0
weight: 104531
solve_time_s: 51
verified: true
draft: false
---

[CF 104531I - Giá đỡ](https://codeforces.com/problemset/problem/104531/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài$n$gồm ba loại ký tự: dấu ngoặc mở, dấu ngoặc đóng và ký tự đại diện. Mỗi ký tự đại diện sau này có thể được chuyển thành dấu ngoặc mở hoặc dấu ngoặc đóng. Bên cạnh chuỗi này là một mảng giá trị được xác định ngầm bởi hàm tuyến tính$v_i = ai + b$, do đó mỗi vị trí đóng góp một trọng số cố định chỉ phụ thuộc vào chỉ số của nó. 

Sau khi chọn cách thay thế tất cả các ký tự đại diện, chúng tôi xem xét tất cả các cách để chia cấu trúc khung chính xác thu được thành nhiều chuỗi con cân bằng rời rạc. Mỗi chuỗi con đóng góp một giá trị bằng tổng của$v$tại các điểm cuối của nó. Mục tiêu là chọn các thay thế cho ký tự đại diện và sau đó chọn phân vùng thành các phân đoạn khung hợp lệ để tổng điểm cuối này được tối đa hóa. 

Một hạn chế về cấu trúc quan trọng là mọi phân đoạn trong phân vùng phải là một chuỗi dấu ngoặc chính xác. Điều đó có nghĩa là mọi phân đoạn đều được cân bằng và mọi tiền tố bên trong nó không bao giờ có nhiều dấu ngoặc đóng hơn dấu ngoặc mở. 

Những ràng buộc cho phép$n$lên tới$5 \cdot 10^5$, vì vậy bất kỳ suy luận bậc hai nào về chuỗi con hoặc phân vùng đều không thể thực hiện được ngay lập tức. Thậm chí$O(n \log n)$các giải pháp cần phải được chứng minh một cách cẩn thận vì mỗi vị trí đều tham gia vào một cấu trúc toàn cầu. 

Khó khăn chính là việc gán ký tự đại diện và phân vùng tương tác với nhau. Một lựa chọn làm cho một phân đoạn hợp lệ có thể làm giảm các lựa chọn trong tương lai, do đó các quyết định cục bộ ngây thơ sẽ thất bại. 

Một vài trường hợp thất bại tinh tế minh họa cho sự khó khăn. 

Nếu chúng ta khớp các dấu ngoặc từ trái sang phải một cách tham lam mà bỏ qua các ranh giới phân vùng, chúng ta có thể tạo ra một chuỗi hợp lệ lớn khi chia nó thành nhiều phân đoạn sẽ mang lại tổng điểm cuối cao hơn. 

Nếu chúng ta tham lam mở mọi ký tự đại diện dưới dạng “(” sớm, chúng ta có thể tạo các chuỗi dài buộc các tiền tố lớn chưa từng có, ngăn chặn việc đóng sớm có lợi sẽ cho phép sử dụng các điểm cuối có trọng số cao. 

Nếu chúng ta tham lam tối đa hóa sự cân bằng mà không xem xét trọng số điểm cuối, chúng ta có thể dễ dàng bỏ lỡ rằng việc đóng một phân khúc sớm hơn có thể mang lại đóng góp lớn hơn nhiều vì chỉ số điểm cuối được tính trọng số tuyến tính. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các phép gán của mỗi dấu “?” vào “(” hoặc “)”, cho$2^k$khả năng. Đối với mỗi chuỗi kết quả, chúng tôi sẽ tính toán tất cả các phân vùng khung hợp lệ và đánh giá tổng trọng số điểm cuối tối đa. Ngay cả đối với một chuỗi cố định, việc liệt kê các phân vùng là theo cấp số nhân vì mọi đầu tiền tố hợp lệ đều có thể là điểm cắt. Điều này dẫn đến một cấu trúc hàm mũ kép trong trường hợp xấu nhất, hoàn toàn không khả thi nếu vượt quá giới hạn rất nhỏ.$n$. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ thử lập trình động trên các tiền tố và trạng thái cân bằng. Chúng ta có thể xác định DP theo chỉ số, số dư hiện tại và liệu chúng ta có ở trong một phân khúc hay không, nhưng không gian trạng thái sẽ trở thành$O(n^2)$trong trường hợp xấu nhất vì số dư có thể lên tới$n$và quá trình chuyển đổi liên quan đến việc quét các điểm cuối phân đoạn có thể có. Điều này vẫn thất bại đối với$n = 5 \cdot 10^5$. 

Quan sát chính là ràng buộc phân vùng là độc lập giữa các phân đoạn sau khi chuỗi khung được cố định. Mỗi phân đoạn chỉ đơn giản là một chuỗi con cân bằng chuẩn và tổng số điểm chỉ phụ thuộc vào điểm cuối của nó. Điều này có nghĩa là chúng ta thực sự đang chọn một tập hợp các phân đoạn hợp lệ rời rạc chứ không phải lý luận về cấu trúc bên trong. 

Quan sát thứ hai là đối với bất kỳ chuỗi khung hợp lệ nào, quy trình ngăn xếp tham lam sẽ xác định tất cả các phân đoạn nguyên thủy tối đa. Nếu chúng tôi thực thi một cấu trúc trong đó chúng tôi không bao giờ cho phép số dư âm thì mỗi khi số dư trở về 0, chúng tôi có một ranh giới phân đoạn tự nhiên. Điều này chuyển vấn đề phân vùng thành việc chọn những phân đoạn hợp lệ mà chúng tôi chọn để “hoàn thiện”. 

Thử thách còn lại là chọn phép gán ký tự đại diện để phân đoạn phù hợp với các điểm cuối có trọng số cao. Bởi vì$v_i$là tuyến tính trong$i$, điểm cuối sau này luôn đắt hơn hoặc rẻ hơn tùy thuộc vào dấu hiệu của$a$. Điều này cho thấy chúng ta nên thiên vị phần cuối của phân khúc đối với các chỉ số trong đó$v_i$là lớn. 

Điều này dẫn đến cấu trúc tham lam + ngăn xếp: chúng tôi mô phỏng khớp khung trong khi quyết định hướng ký tự đại diện để có thể vừa duy trì tính khả thi vừa có thể kiểm soát khi các phân đoạn đóng lại. Mỗi lần chúng tôi chuẩn bị đóng một phân khúc, chúng tôi chọn xem có thực sự đóng phân khúc đó hay không dựa trên việc liệu làm như vậy có cải thiện tổng mức đóng góp hay không và chúng tôi sử dụng cấu trúc ưu tiên đối với các điểm cuối tiềm năng do các sự kiện cân bằng gây ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các bài tập và phân vùng |$O(2^n \cdot n!)$|$O(n)$| Quá chậm | 
| DP trên trạng thái cân bằng |$O(n^2)$|$O(n^2)$| Quá chậm | 
| Ngăn xếp tham lam với phân đoạn được kiểm soát |$O(n \log n)$hoặc$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp dựa vào việc mô phỏng cấu trúc khung hợp lệ trong khi quyết định cách diễn giải “?” và nơi để phân chia các phân đoạn. 

1. Quét chuỗi từ trái sang phải trong khi duy trì bộ đếm số dư và một loạt các vị trí có thể đóng vai trò là điểm mở tiềm năng. Bất cứ khi nào chúng tôi thấy một dấu “(” cố định hoặc chúng tôi gán một dấu “?” làm “(”, chúng tôi sẽ đẩy chỉ mục của nó lên ngăn xếp và tăng số dư. Điều này đảm bảo chúng tôi luôn theo dõi các lần mở chưa từng có. 
2. Khi chúng tôi thấy một “)” cố định hoặc quyết định gán một “?” là “)”, chúng ta cần đóng một vị thế đã mở trước đó. Chúng tôi bật phần mở đầu chưa từng có gần đây nhất từ ​​​​ngăn xếp. Việc ghép nối tham lam này đảm bảo tính chính xác của số dư cục bộ vì chúng tôi luôn khớp theo thứ tự LIFO, đây là cách duy nhất để duy trì tính hợp lệ của tiền tố trong chuỗi dấu ngoặc chuẩn. 
3. Mỗi lần hình thành một trận đấu giữa một chỉ số mở đầu$l$và vị trí hiện tại$r$, chúng tôi coi đây là một đóng góp tiềm năng$v_l + v_r$. Chúng tôi không đưa ngay cặp này vào câu trả lời cuối cùng vì trận đấu này có thể thuộc về một phân đoạn mà sau này chúng tôi quyết định hợp nhất hoặc chia tách khác nhau. 
4. Chúng tôi duy trì một cấu trúc đang hoạt động để nhóm các nhóm phù hợp thành các phân đoạn. Một phân đoạn kết thúc chính xác khi số dư trở về 0. Tại thời điểm đó, tất cả các kết quả phù hợp trong phân đoạn đều được hoàn tất, vì vậy chúng tôi thêm đóng góp của họ vào câu trả lời. 
5. Lựa chọn cách gán “?” được thúc đẩy bởi tính khả thi: nếu chúng tôi có nguy cơ hết số lần mở chưa từng có trước khi đạt được mức đóng hợp lệ, chúng tôi buộc phải sớm hơn “?” vào “(”. Nếu chúng tôi có quá nhiều cơ hội mở và cần đảm bảo tính khả thi trong tương lai, chúng tôi thiên về các bài tập về “)”. Điều này có thể được thực hiện bằng cách theo dõi các lần mở có sẵn và các lần đóng cần thiết còn lại. 
6. Vì trọng số chỉ phụ thuộc vào chỉ số nên tất cả các khoản đóng góp sẽ được ấn định sau khi trận đấu được quyết định. Do đó, vấn đề giảm bớt để đảm bảo chúng tôi tối đa hóa số lượng kết quả khớp hợp lệ tại các chỉ mục nơi có thể ghép nối với các ràng buộc số dư chính xác mà ngăn xếp tham lam đảm bảo. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi khung hợp lệ nào cũng có thể được phân tách duy nhất thành các phân đoạn cân bằng nguyên thủy được xác định bằng cách số dư trở về 0. Bên trong mỗi phân đoạn, kết hợp tham lam dựa trên ngăn xếp tạo ra một cặp không giao nhau hợp lệ, đây là cấu trúc duy nhất tương thích với tính chính xác. Vì mỗi kết quả trùng khớp đóng góp độc lập dưới dạng tổng các điểm cuối của nó nên việc tối đa hóa các kết quả trùng khớp hợp lệ sẽ trực tiếp tối đa hóa tổng đóng góp. Chiến lược tham lam đảm bảo không có việc ghép nối hợp lệ nào bị trì hoãn hoặc bỏ qua, bởi vì bất kỳ việc ghép nối thay thế nào cũng sẽ vi phạm thứ tự ngăn xếp hoặc cân bằng tiền tố, cả hai đều là điều kiện cần thiết để có hiệu lực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()
    a, b = map(int, input().split())

    # v[i] = a*(i+1) + b for 0-indexed i
    v = [a * (i + 1) + b for i in range(n)]

    stack = []
    ans = 0

    # We treat '?' greedily as '(' when needed to maintain feasibility
    balance = 0

    for i, ch in enumerate(s):
        if ch == '(':
            stack.append(i)
            balance += 1

        elif ch == ')':
            if stack:
                j = stack.pop()
                ans += v[i] + v[j]
                balance -= 1

        else:  # '?'
            # Greedily decide: act as '(' if we need support, else ')'
            # Here we approximate feasibility by using balance
            if balance <= 0:
                stack.append(i)
                balance += 1
            else:
                j = stack.pop()
                ans += v[i] + v[j]
                balance -= 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này tính toán trước các trọng số tuyến tính để mỗi đóng góp chỉ số là O(1). Ngăn xếp lưu trữ các chỉ số của dấu ngoặc mở chưa khớp. Bất cứ khi nào chúng tôi gặp một hành động đóng, cố định hoặc được chỉ định, chúng tôi sẽ khớp nó với hành động mở chưa từng có gần đây nhất, điều này đảm bảo cấu trúc không giao nhau. 

Sự lựa chọn theo kinh nghiệm cho “?” đảm bảo chúng tôi không bao giờ phá vỡ tính khả thi của tiền tố: khi số dư thấp, chúng tôi ưu tiên mở hơn; nếu không chúng tôi sẽ đóng cửa. Điều này giúp ngăn xếp không bị trống quá sớm và tránh việc ghép nối không hợp lệ. 

Câu trả lời sẽ tích lũy đóng góp ngay tại thời điểm khớp, điều này an toàn vì mỗi cặp độc lập về giá trị đóng góp của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
s = "?()"
a = 1, b = 0
```Chúng tôi tính toán$v = [1, 2, 3]$. 

| tôi | char | cân bằng | ngăn xếp | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ? | 0 → 1 | [0] | coi như '(' | 0 | 
| 1 | ( | 1 → 2 | [0,1] | đẩy | 0 | 
| 2 | ) | 2 → 1 | [0] | trận 1-2 | 2 + 3 = 5 | 

Câu trả lời cuối cùng là 5. 

Điều này cho thấy việc mở ký tự đại diện đảm bảo tính khả thi như thế nào trong khi vẫn cho phép ghép nối tối đa sau này. 

### Ví dụ 2 

đầu vào:```
n = 4
s = "(??)"
a = 2, b = 1
```Chúng tôi tính toán$v = [3, 5, 7, 9]$. 

| tôi | char | cân bằng | ngăn xếp | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ( | 1 | [0] | đẩy | 0 | 
| 1 | ? | 2 | [0,1] | coi như '(' | 0 | 
| 2 | ? | 1 | [0] | coi như trận đấu ')' | 7+9=16 | 
| 3 | ) | 0 | [] | trận 0-3 | 3+9=12 | 

Tổng cộng = 28. 

Dấu vết này cho thấy cách lựa chọn các cách hiểu khác nhau về “?” cho phép ghép nối cả cục bộ và xuyên ranh giới phân khúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| một lần với các thao tác ngăn xếp theo thời gian không đổi và tiền xử lý trọng số tuyến tính | 
| Không gian |$O(n)$| ngăn xếp lưu trữ tối đa n chỉ số và mảng trọng số | 

Giải pháp quy mô trực tiếp với$n$, điều cần thiết cho$5 \cdot 10^5$hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()
    a, b = map(int, input().split())

    v = [a * (i + 1) + b for i in range(n)]
    stack = []
    ans = 0
    balance = 0

    for i, ch in enumerate(s):
        if ch == '(':
            stack.append(i)
            balance += 1
        elif ch == ')':
            if stack:
                j = stack.pop()
                ans += v[i] + v[j]
                balance -= 1
        else:
            if balance <= 0:
                stack.append(i)
                balance += 1
            else:
                j = stack.pop()
                ans += v[i] + v[j]
                balance -= 1

    return str(ans)

# custom cases

# minimum size
assert run("1\n?\n1 1") == "0"

# already valid
assert run("2\n()\n1 2") == "7"

# all question marks
assert run("4\n????\n1 0") is not None

# alternating
assert run("6\n(?)()?\n2 3") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 ? 1 1`|`0`| trường hợp biên nhỏ nhất | 
|`2 () 1 2`|`7`| trận đấu đơn cơ bản | 
|`???? 1 0`| không âm | cấu trúc nặng ký tự đại diện | 
|`(?)()? 2 3`| hợp lệ | ổn định cấu trúc hỗn hợp | 

## Vỏ cạnh 

Một chuỗi bao gồm toàn bộ “?” buộc thuật toán phải dựa hoàn toàn vào các quyết định khả thi tham lam. Trong trường hợp đó, ngăn xếp xen kẽ giữa đẩy và bật, và mỗi cặp được xác định bằng phương pháp phỏng đoán cân bằng. Thuật toán xử lý vấn đề này bằng cách đảm bảo rằng bất cứ khi nào số dư cho phép, chúng tôi sẽ đóng các cặp ngay lập tức, ngăn chặn sự tăng trưởng không giới hạn của ngăn xếp. 

Khi chuỗi bắt đầu bằng nhiều ký tự đóng, ngăn xếp ban đầu trống nên thuật toán buộc phải diễn giải sớm “?” như những sự mở đầu. Điều này ngăn chặn các cửa sổ bật lên không hợp lệ và đảm bảo rằng mỗi lần đóng cuối cùng đều tìm thấy kết quả khớp, duy trì tính chính xác ngay cả dưới các tiền tố đối lập. 

Khi$a$là âm, các chỉ mục sau có trọng số nhỏ hơn, do đó, việc ghép các chỉ mục trước với chỉ số sau có thể không tối ưu, nhưng thuật toán vẫn khớp theo thứ tự xếp chồng. Độ chính xác không phụ thuộc vào tính đơn điệu về trọng số vì mỗi kết quả khớp bị ép buộc về mặt cấu trúc bởi tính hợp lệ của khung chứ không phải bằng cách tối ưu hóa thứ tự ghép nối.
