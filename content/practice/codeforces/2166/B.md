---
title: "CF 2166B - Đóng tab"
description: "<Giá trị tiêu đềa). Nếu hiện có các tab (=\"Solution Editorial\"m) đang mở thì mỗi tab đều có độ dài [ size=\"2xl\"/<Title value=\"Problem Hiểu\" size=\"text{len} = minleft(b,frac{a}{m}right)."
date: "2026-06-09T04:30:38+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2166
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1064 (Div. 2)"
rating: 900
weight: 2166
solve_time_s: 289
verified: false
draft: false
---

[CF 2166B - Đóng tab](https://codeforces.com/problemset/problem/2166/B) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 4 phút 49 giây 
**Đã xác minh:** không 

## Giải pháp 
<Giá trị tiêu đềa\). Nếu hiện tại có\(="Solution Editorial"m\)các tab đang mở, mỗi tab đều có độ dài$$
 size="2xl"/><Title value="Problem Understanding" size="\text{len} = \min\left(b,\frac{a}{m}\right).
$$xl"/><Text>Có <Text inline Weight="semibold" value="m"/>Các tab được sắp xếp từ trái sang phải nên nút đóng của chúng nằm ở$$
\text{ tabs currently open. Every tab has the same length <Mathlen},\ 2\text{len},\ 3\text{len},\ \dots,\ m\text{len}.
$$Khi value="\text{len}=\min(b,\frac{a}{m chúng tôi đóng một tab, số lượng tab còn lại sẽ giảm, điều này làm thay đổi độ dài của tab và do đó thay đổi tất cả các lần đóng trong tương lai})"/>. Các nút đóng (x&apos;s) là các vị trí nút. 

Con trỏ bắt đầu ở vị trí\(0\). Chúng ta có thể di chuyển nó tại các vị trí <Math value="\text{len},2\text{len},3\text{len},\dots,m tới bất kỳ vị trí nào. Khi con trỏ ở vị trí nào đó\(x\), chúng ta có thể\text{len}"/> được đo từ cạnh trái liên tục nhấp chuột bất cứ khi nào nút đóng xuất hiện chính xác\(x\). Câu hỏi yêu cầu số lần di chuyển chuột tối thiểu cần thiết để đóng toàn bộ màn hình.</Text><Text>Khi bạn đóng các tab,\(n\)tab. 

Những hạn chế là rất lớn. Cả hai\(a\)Và\(n\)có thể đạt mức \(10^9\ <Text inline Weight="semibold" value="m"/> giảm, vì vậy tất cả các vị trí tab có thể) và có tới\(10^4\)trường hợp thử nghiệm. Bất kỳ sự mô phỏng nào của quá trình đóng đều là sự thay đổi. Con trỏ của bạn bắt đầu ở vị trí <Text inline Weight="semibold" value="0"/>. Một điều không thể. Chúng ta cần một công thức thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Phần khó khăn là hiểu rằng chuyển động của chuột có nghĩa là di chuyển con trỏ đến vị trí mới; Khi đó, một vị trí chuột duy nhất có thể vẫn hữu ích trong nhiều lần đóng liên tiếp vì thao tác đóng có thể nhấp liên tục mà không cần di chuyển thêm.</Text><Text>Nhiệm vụ là tìm nút của tab ngoài cùng bên phải có thể xuất hiện nhiều lần ở cùng một tọa độ. 

Hãy xem xét: 

`` the minimum number of cursor movements needed to close all tabs.</Text><Text>The constraints are very small`a = 8, b = 1, n = 6```

For every \(m \ computationally but very large numerically: <Mathge 8\),

\[
\min(b,a/m)=1.
\]

 value="a,b,n \le 10^9"/> and up to <Math value="10^The rightmost close button is always at position \(m\cdot 1\), which changes. However, when \(m<4"/> test cases. This rules out any simulation that closes tabs one by one. We need an <8\),

\[
\text{len}=a/m,
\]

and the rightmost close button becomes

\Math value="O(1)"/> or <Math[
m\cdot (a/m)=a=8.
\]

A careless simulation may miss that many future clicks value="O(\log n)"/> solution per test case.</Text><Title value="Non-obvious can happen at the same coordinate.

Another subtle case is

```a = 9, b = 6, n = 2 
`` edge cases" size="lg"/><Card size="`Vì\(m=2\),$$
\text{len}=\min(6,4.full" background="surface" gap={3}><5)=4.5.
$$Các nút đóng nằm ở\(4.5\)Và\(9\). AfterTitle value="Vị trí nút đóng có thể cố định khi đóng một tab,\(m=1\)và nút đóng duy nhất là ở\(6\). Không có một lần xóa nhiều lần" size="sm"/><Text>Vị trí ví dụ phù hợp với cả hai trạng thái, vì vậy câu trả lời là\(2\). 
## Phương pháp tiếp cận 
Đáp:</Text><Code Block ngôn ngữ="text" content="a = 8, b = cách diễn giải thô bạo sẽ theo dõi vị trí của tất cả các nút đóng trong 1, n = 6"/><Text>Đối với mọi < mọi giá trị của\(m\), sau đó thử xác định vị trí nào canMath value="m \ge 8"/>, độ dài tab < được sử dụng lại. Ngay cả đối với một trường hợp thử nghiệm duy nhất,\(n\)Có lẽ\(Math value="1"/>, so the rightmost close button stays10^9\), vì vậy việc xử lý mọi trạng thái là vô vọng. 

Quan sát chính xuất phát từ việc nhìn vào tab ngoài cùng bên phải. 

tại vị trí <Giá trị toán học="1"/>. Bạn có thể di chuyển một lần đến vị trí Khi có\(m\)các tab, nút đóng ngoài cùng bên phải là ở$$
R(m)=m\cd <Math value="1"/> and click six times. The answer is <Text inline weightot \min\left(b,\frac{a}{m}\right).
$$Điều này đơn giản hóa ngay lập tức:$$
R(m)=="semibold" value="1"/>. A naive approach that assumes positionsmin(mb,a).
$$Giả sử chúng ta đặt con trỏ ở một vị trí nào đó\( always change would incorrectly count multiple movements.</Text></Card><Card size="full"x\). Chúng ta có thể tiếp tục nhấp chuột miễn là nút đóng ngoài cùng bên phải vẫn ở vị trí\(x\). nền="surface" khoảng cách={3}><Title value="Vị trí nút đóng có thể thay đổi Do đó, mỗi giá trị riêng biệt của \(R(m)\) yêu cầu một lần di chuyển chuột. 

Bây giờ hãy kiểm tra sau mỗi lần xóa" size="sm"/><Text>Ví dụ:</Text><CodeBlock ngôn ngữ="text" trình tự$$
R(n),R(n-1),\dots,R(1).
$$Nếu \(mb<a\ content="a = 9, b = 6, n = 2"/><Text>Với), thì$$
R(m)=mb.
$$Những giá trị này đều khác biệt vì\(b> two tabs, <Math value="\text{len}=9/2=4.5"/0\). 

Nếu như\(mb\ge a\), sau đó$$
R(m)=a.
$$All>, do đó nút đóng ngoài cùng bên phải có giá trị <Giá trị toán học, các trạng thái này có cùng vị trí. 

Do đó trình tự bao gồm:$$
="9"/>. After closing one tab, <Math valuea,a,a,\dots,a
$$cho tất cả\(m\ge \lceil a/b\rceil="\text{len}=6"/>, so the remaining close button is at <\), theo sau là$$
(\lceil a/b\rceil-1)b,
(\lceil a/bMath value="6"/>. The cursor must move from <Math\rceil-2)b,
\dots,
b.
$$Mỗi giá trị dưới đây\( value="9"/> to <Math value="6"/>, so the answer isa\)là duy nhất 

Cho phép$$
k=\left\lceil \frac{a}{ <Text inline weight="semibold" value="2"/>. A careless solution that only checks theb}\right\rceil.
$$Nếu như\(n<k\), thì mọi trạng thái đều thỏa mãn\(mb<a\), cấu hình ban đầu sẽ bỏ lỡ điều này.</Text></Card>< vì vậy tất cả\(n\)vị trí là khác biệt và câu trả lời là\(n\). 

Nếu như\(n\Card size="full" background="surface" gap={3}><Title value="More tabs thange k\), thì tất cả các trạng thái với\(m\ge k\)chia sẻ một vị trí\(a fit at the maximum length" size="\), trong khi phần còn lại\(k-1\)tiểu bang đóng góp\(k-1\)sm"/><Text>Ví dụ:</Text><CodeBlock ngôn ngữ="text" content="a = 8, b = 1, n = 9"/>< vị trí riêng biệt. Câu trả lời trở thành$$
1+(k-1)=k.
$$Kết hợp cả hai trường hợp,$$
\boxed{\Text>When <Math value="m \ge 8text{answer}=\min\left(n,\left\lceil\"/>, thefrac{a}{b}\right\rceil\right)}.
$$### Nút đóng ngoài cùng bên phải ở mức <Math value So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(n)\) | \(O(1)\) | Quá chậm | 
| Tối ưu | \(O(1)\) | \(O(1)\) | Đã chấp nhận="1"/>. Sau khi xóa đủ các tab, <Math value="m"/> trở thành <Math value="7"/> và bây giờ là <Math value=" | 

## Hướng dẫn thuật toán 

1. Tính toán$$
k=\left\lceil \frac{a}{b}\right\rceil.
$$2. Quan sát điều đó với mọi trạng thái với\(m\gea/m &gt; 1"/>, so the close button k\), nút đóng ngoài cùng bên phải nằm ở vị trí\(a\). 

3. Quan sát điều đó với mọi trạng thái với\(m<k\), vị trí bên phải sẽ thay đổi. Câu trả lời trở thành <Text inline Weight="semibold" value="nút đóng gần nhất nằm ở\(mb\)và tất cả các vị trí như vậy2"/> chứ không phải <Text inline Weight="semibold" value="1"/>.</Text></Card><Title value=" là khác biệt. 

4. Nếu\(n<k\), mỗi bang đóng góp một vị trí khác nhau nên câu trả lời là\(n\). 

5. Nếu\(n\ge k\),Approaches" size="xl"/><Text>Mô phỏng mạnh mẽ sẽ liên tục tính toán lại độ dài tab và đóng tất cả các trạng thái ngoài cùng bên phải bằng\(m\ge k\)sụp đổ vào một vị trí chung\(a\), trong khi phần còn lại\(k-1\)sau mỗi lần xóa, đếm xem có bao nhiêu trạng thái riêng biệt đóng góp vào các vị trí riêng biệt. Câu trả lời là\(k\). 

6. Đầu ra$$
\min positions the cursor must visit. This works because the cursor(n,k).
$$### Tại sao nó hoạt động 

Nút đóng duy nhất quan trọng là nút ngoài cùng bên phải. Việc đóng chỉ cần di chuyển khi vị trí của nút đóng ngoài cùng bên phải thay đổi. Tuy nhiên, với tab <Math sẽ giảm số lượng tab xuống đúng một và chuyển chúng ta từ trạng thái\( value="n"/> up to <Math value="10^9"/m\)để nêu\(m-1\). 

Đối với tiểu bang\(m\), nút đóng ngoài cùng bên phải được đặt>, việc mô phỏng mọi thao tác xóa là không thể.</Text><Text>Quan sát quan trọng là tại$$
R(m)=\min(mb,a).
$$Mỗi khi hai vị trí nút đóng ngoài cùng bên phải khi có các tab <Math value="m"/> có</ trạng thái khác nhau có cùng giá trị \(R(m)\), chúng có thể được xử lý mà không cầnText><Math block value="x(m)=m\cdot \min\ di chuyển chuột. Mỗi khi giá trị thay đổi, một vị trí chuột mớileft(b,\frac{a}{m}\right)=\min(mb,a)."/><Text>Như là bắt buộc. 

Các giá trị bằng\(a\)tạo thành một khối lớn. Mọi giá trị bên dưới chúng ta đóng các tab, <Math value="m"/> giảm từ <Math\(a\)là bội số phân biệt của\(b\). Đếm các giá trị riêng biệt của \(R(m)\) trên\(m value="n"/> down to <Math value="1"/>.</=1,\dots,n\)đưa ra chính xác$$
\min\left(n,\left\lceil\frac{a}{b}\rightText><Text>There are only two regimes:</Text><List\rceil\right).
$$## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t marker="decimal" connector="solid" = int(input())
    ans = []

    for _ in range(t):
        a, b, n = map(int, input().split())

        gap={3}><List.Item><Text><Text inline weight k = (a + b - 1) // b  # ceil(a / b)
        ans.append(str(min(n, k)))

   ="semibold" value="Saturated regime:"/> If <Math sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc triển khai gần như là value="mb \ge a"/>, sau đó là <Math value="x(m)=a"/>. hoàn toàn là công thức toán học. 

biểu thức```python
(a + b - 1) // b
 The close button stays at the fixed position <Math value```tính toán="a"/>.</Text></List.Item><List.Item><Text><Text inline Weight="semibold" value="Chế độ tuyến tính:"/> Nếu <Math value="mb &lt; a"/> thì <Math value="x\[
\left\lceil \frac{a}{b}\right\rceil
\]sử dụng số học số nguyên. 

Câu trả lời cuối cùng chỉ đơn giản là```python
min(n, k)
```phù hợp trực tiếp với công thức dẫn xuất. 

No(m)=mb"/>. Cần có các giá trị khác nhau của <Math số học dấu phẩy động để tránh các vấn đề về độ chính xác khi\(a\)Và\( value="m"/> give different positions.</Text></List.Item></List><Text>Define <b\)lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 Math value="t=\left\lceil \frac1 6
```Đây\[
k=\left\lceil\frac{8}{1}\right\rceil={a}{b}\right\rceil"/>. Then <Math value="mb \8.
\]| m | R(m)=min(mb,a) | 
|---|---| 
| 6 | ge a"/> chính xác khi <Math value="m \ge t"/>.</Text><Text6 | 
| 5 | 5 | 
| 4 | 4 | 
| 3 | 3 | 
| 2 | 2 | 
>Nếu <Giá trị toán học="n &lt; t"/| 1 | 1 | 

Có sáu vị trí riêng biệt. 

Kể từ khi\(n<k\), 

\>, chúng ta luôn ở chế độ tuyến tính nên mỗi lần xóa sẽ thay đổi [ 
\text{answer}=6. 
\] 

Tuy nhiên, chúng ta có thể giữ vị trí và chúng ta cần các chuyển động <Math value="n"/>.</Text><Text>Nếu <Math value="n \ con trỏ ở vị trí\(1\)và liên tục đóng tab hiển thị ngoài cùng bên trái asge t"/>, sau đó tất cả các trạng thái có bố cục <Math value="m=t,t+1,\dots đều co lại. Quan sát dự định của vấn đề là sự khác biệt,n"/> có cùng vị trí <Math value="a"/>. Chúng ta có thể xử lý các trạng thái ngoài cùng bên phải thu gọn vào công thức, mang lại kết quả\[
\min(6,8)=1.
 all of them with one movement. After that, for <Math value="m=t-1,t-\]Đầu ra giải pháp được chấp nhận\(1\). 

### Ví dụ 2 

đầu vào:```
9 6 2,\dots,1"/>, the positions are distinct, requiring2
```Đây\[
k=\left\lceil\frac{9}{6}\right\rceil= <Math value="t-1"/> more movements. Total: <Math value="1+(2.
\]| m | R(m)=min(mb,a) | 
|---|---| 
| 2 | 9 | 
t-1)=t"/>.</Text><Text>Vậy câu trả lời đơn giản là| 1 | 6 | 

Các vị trí là khác nhau. 

Như vậy\[
\text{answer}=\</Text><Math block value="\boxed{\min\left(n,\min(2,2)=2.
\]Lần nhấp chuột đầu tiên có thể xảy ra lúc\(9\), butleft\lceil \frac{a}{b}\right\rceil\right)}."/><Table>< sau khi còn lại một tab, nút đóng sẽ di chuyển đến\(6\), buộc một chuyển động chuột khác. 

## ComplexityTable.Row><Table.Cell><Text Weight="semibold">Phương pháp tiếp cận</Text></Table Phân tích 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(1)\) mỗi trường hợp thử nghiệm | Only.Cell><Table.Cell><Text Weight="semibold">Độ phức tạp về thời gian</Text></Table.Cell><Table.Cell><Text một vài phép tính số học | 
| Không gian | \(O(1)\) | Không có cấu trúc dữ liệu bổ sung | 

Với mức trọng lượng="semibold">Độ phức tạp của không gian</Text></Table.Cell><Table nhất\(10^4\)trường hợp thử nghiệm, tổng công việc là rất nhỏ. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

.Cell><Text Weight="semibold">Phán quyết</Text></Table.Cell></Table.Row><Table.Row><Table.Cell>Brute Force</Table.Cell><Table.Cell><Math value="O## trường hợp thử nghiệm```python(n)"/></Table.Cell><Table.Cell><Math value="O
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

   (1)"/></Table.Cell><Table.Cell>Too slow for <Math for _ in range(t):
        a, b, n = map(int, input().split())
        k = (a + b - 1) // b
 value="n=10^9"/></Table.Cell></Table.Row><Table.Row><Table.Cell>        out.append(str(min(n, k)))

    return "\n".join(out)

# provided sample
assert run("""12
8 Optimal</Table.Cell><Table.Cell><Math value="O(1)"/></Table.Cell><Table.Cell><Math1 6
9 6 2
10 3 1
10 1 10
9 2 1
5 5  value="O(1)"/></Table.Cell><Table.Cell>Accepted</Table.Cell></Table.Row></Table><6
6 2 7
9 1 9
3 2 6
8 1 7
8 1 9
8 2 Title value="Algorithm Walkthrough" size="xl"/><List marker="decimal" connector="solid" gap={4
""") == """1
2
1
1
1
1
2
1
2
1
2
1"""

#3}><List.Item>Compute <Math value="t=\left\lceil \frac minimum values
assert run("""1
1 1 1
""") == "1"

# large values{a}{b}\right\rceil"/>. In
assert run("""1
1000000000 1 1000000000
""") == "2"

# b = integer arithmetic, <Math value="t=(a+b-1)//b"/>.</ a
assert run("""1
10 10 100
""") == "1"

# boundaryList.Item><List.Item>If <Math value="n & around ceil(a/b)
assert run("""1
10 3 4
""") == "1"
```### Tóm tắt thử nghiệm tùy chỉnh 

lt; t"/>, mọi trạng thái đều ở chế độ tuyến tính <| Đầu vào thử nghiệm | Đầu ra dự kiến | Những gì nó xác nhận | 
|---|---|---| 
|`1 1 1`|`1`|Math value="x(m)=mb"/>, do đó tất cả các vị trí đều khác biệt. Đáp án là <Toán Trường hợp nhỏ nhất | 
|`1000000000 1 1000000000`| kết quả công thức | value="n"/>.</List.Item><List.Item>Nếu <Math value="n \ge t Giá trị lớn nhất | 
|`10 10 100`|`1`| Tất cả các tab luôn"/>, khi đó các vị trí của <Math value="m=t,t+1,\dots,n"/> có cùng vị trí đóng | 
|`10 3 4`| trường hợp ranh giới | tất cả đều bằng <Math value="a"/>, yêu cầu một chuyển động. Chuyển tiếp xung quanh\(\lceil a/b\rceil\)| 

## Vỏ cạnh 

Hãy xem xét:```
a= The remaining <Math value="t-1"/> positions are distinct, so the total is <5, b=5, n=6
```Chúng tôi có\[
\left\Math value="t"/>.</List.Item><List.Item>Return <Math value="\min(n,t)lceil \frac{5}{5}\right\rceil=1.
\]Mọi tiểu bang đều đã đáp ứng\(mb\"/>.</List.Item></List><Text weight="semibold">Why it works.</Text><Textge a\), do đó mọi nút đóng ngoài cùng bên phải đều ở vị trí\(5\). Thuật toán xuất ra \(1\>Vị trí nút đóng ngoài cùng bên phải là <Math value="x(m)=\min(mb,a)"/>). 

Coi như:```
a=8, b=1, n=9
```Chúng tôi có\[
\left\. As <Math value="m"/> decreases, this sequence is constant while <Math value="m \ge t"/>, then strictly decreaseslceil \frac{8}{1}\right\rceil through the values <Math value="(t-1)b,(t-=8.
\]Hoa với\(m\ge8\)chia sẻ vị trí\(8\). Các trạng thái còn lại tương ứng với bội số riêng biệt của\(1\). Công thức cho\[
\min2)b,\dots,b"/>. The minimum number of movements equals(9,8)=8.
\]Đây chính xác là số lượng vị trí nút ngoài cùng bên phải riêng biệt. 

Coi như:```
 the number of distinct positions in this sequence, which is exactly <Math value="\a=9, b=2, n=1
```Chỉ tồn tại một trạng thái đóng tab. Bất kểmin(n,t)"/>.</Text><Title value="Python Solution" size="xl"/>< của hình học, một vị trí chuột là đủ. Thuật toán tính toán\[
\CodeBlock language="python" content="import sys&#10;input =min\left(1,\left\lceil\frac92\right\rceil\right)=1.
\]Kết quả đầu ra sys.stdin.readline&#10;&#10;t = int(input())&#10;out =
