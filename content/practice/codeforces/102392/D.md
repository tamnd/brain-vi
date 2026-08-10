---
title: "CF 102392D - Chuỗi chu kỳ?"
description: "Đặt độ dài đầu vào là (L=2n). Đầu vào là nhiều tập hợp các chữ cái viết thường, vì thứ tự tuần hoàn ban đầu đã bị hủy và chỉ còn lại các ký hiệu. Chúng ta phải sắp xếp lại các chữ cái đó thành một chu trình sao cho các chuỗi con tuần hoàn (L) có độ dài (n) đều khác nhau."
date: "2026-08-10T19:28:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102392
codeforces_index: "D"
codeforces_contest_name: "2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)"
rating: 0
weight: 102392
solve_time_s: 112
verified: true
draft: false
---

[CF 102392D - Chuỗi chu kỳ?](https://codeforces.com/problemset/problem/102392/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đặt độ dài đầu vào là (L=2n). Đầu vào là nhiều tập hợp các chữ cái viết thường, vì thứ tự tuần hoàn ban đầu đã bị hủy và chỉ còn lại các ký hiệu. Chúng ta phải sắp xếp lại các chữ cái đó thành một chu trình sao cho các chuỗi con tuần hoàn (L) có độ dài (n) đều khác nhau. Một chuỗi con có thể vượt qua phần cuối của chuỗi được in, do đó các ký tự cuối cùng và ký tự đầu tiên thuộc cùng một chu kỳ. Tuyên bố chính thức xác nhận rằng có chính xác (2n) cửa sổ tuần hoàn để phân biệt. 

Khó khăn chính là chuỗi có thể chứa tối đa (10^6) ký tự. Một thuật toán kiểm tra mọi hoán vị có thể là hoàn toàn không thể và ngay cả một thuật toán so sánh rõ ràng tất cả (2n) cửa sổ theo từng ký tự cũng quá đắt nếu lặp lại nhiều lần. Bảng chữ cái chỉ có 26 chữ cái, đây là tham số nhỏ hữu ích ở đây. Chúng ta có thể đếm từng ký tự trong một lần truyền và sau đó thực hiện việc xây dựng từ 26 tần số đó. 

Có một số trường hợp nhỏ khi việc xây dựng có vẻ hợp lý lại thất bại. Vì`aa`, chiều dài là bốn? Không, ở đây (L=2), vì vậy độ dài cửa sổ yêu cầu là một. Cả hai cửa sổ tuần hoàn đều có cùng một chữ cái, tạo nên câu trả lời`NO`. Vì`aaaa`, (L=4) và độ dài cửa sổ yêu cầu là hai. Mọi sự sắp xếp đều chứa hai liên tiếp`a`ký tự, và trên thực tế là cửa sổ hai chữ cái`aa`xảy ra nhiều lần, vì vậy câu trả lời là`NO`. 

Trường hợp ranh giới`aabb`là khác nhau. chu kỳ`aabb`có cửa sổ`aa`,`ab`,`bb`, Và`ba`, vậy cả bốn đều khác biệt và câu trả lời là`YES`. Một quy tắc bất cẩn như "một ký tự lặp lại khiến việc xây dựng không thể thực hiện được" sẽ bác bỏ trường hợp hợp lệ này. 

Một trường hợp tế nhị khác là`aaaabb`, với độ dài sáu và độ dài cửa sổ yêu cầu là ba. Có bốn bản sao của`a`và hai bản sao của`b`. Không có sự sắp xếp nào có thể tránh được việc lặp lại một cửa sổ dài ba chiều, vì vậy câu trả lời là`NO`. Ngược lại,`aaaabc`có bốn`a`ký tự và hai chữ cái còn lại khác nhau. Sự sắp xếp`aabaac`có cửa sổ`aab`,`aba`,`baa`,`aac`,`aca`, Và`caa`, tất cả đều khác biệt nên trường hợp biên này phải được chấp nhận. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ liệt kê mọi hoán vị của ký hiệu (L) và kiểm tra xem các cửa sổ có độ dài chu kỳ-(n) của nó có khác biệt hay không. Nếu chúng ta coi các ký hiệu bằng nhau là khác biệt trong quá trình liệt kê thì sẽ có các ứng cử viên (L!). Đối với mỗi ứng cử viên, có (L) cửa sổ tuần hoàn và so sánh từng ký tự cửa sổ theo chi phí ký tự (n), đưa ra các thao tác ký tự (O(L! \cdot L \cdot n)=O(L!L^2)). Với (L=10^6), ngay cả việc tạo ra các ứng viên cũng không được xem xét. Việc coi các chữ cái giống nhau là không thể phân biệt được sẽ làm giảm số lượng ứng viên, nhưng nó vẫn mang tính giai thừa trong trường hợp xấu nhất. 

Quan sát hữu ích là chúng ta không thực sự cần tìm kiếm một sự sắp xếp. Câu trả lời gần như bị chi phối hoàn toàn bởi tần số của ký tự phổ biến nhất. Gọi tần số của nó là (m). Khi (m\leq n=L/2), chỉ cần sắp xếp toàn bộ chuỗi là đủ. Khi (m) lớn hơn (n), khối lớn của ký tự đó phải được tách ra một cách có chủ ý. Chỉ có một vài dải tần có thể có và mỗi dải có cấu trúc trực tiếp. Đặc tính dựa trên tần số này là ý tưởng trung tâm đằng sau giải pháp được chấp nhận. Một giải pháp được công bố cho bài toán cuộc thi sử dụng chính xác những trường hợp này, bao gồm cả ranh giới ngoại lệ (m=L-2). 

Lý do cấu trúc được sắp xếp hoạt động khi (m\leq n) là vì không có một ký tự nào có thể chiếm hơn một nửa chu kỳ. Trong chu trình được sắp xếp, mỗi cửa sổ bắt đầu ở một vị trí khác nhau có kiểu chuyển tiếp khác nhau giữa các lần chạy ký tự. Một cửa sổ có độ dài-(n) lặp lại sẽ buộc hai vị trí bắt đầu nhìn thấy cùng một ranh giới chạy cho toàn bộ cửa sổ, điều này sẽ yêu cầu một ký tự chạy dài hơn (n). Giới hạn tần số loại trừ điều đó. 

Khi (m>n), việc sắp xếp sẽ tạo ra một chuỗi quá dài, vì vậy chúng tôi chia nhân vật chính xung quanh một nhân vật khác. Nếu (m\leq L-3), lấy (n-1) bản sao của ký tự chính, đặt một ký tự khác sau chúng, đặt các bản sao chính còn lại sau đó và nối tất cả các chữ cái còn lại. Dải phân cách duy nhất được đặt có chủ ý sẽ phá vỡ thời gian dài ở chính xác vị trí mà các cửa sổ tuần hoàn sẽ va chạm vào nhau. 

Trường hợp (m=L-2) là ranh giới khả thi chặt chẽ nhất có thể. Chỉ có hai biểu tượng vẫn nằm ngoài ký tự chi phối. Nếu chúng khác nhau, việc đặt (n-1) ký tự trội, ký tự thiểu số đầu tiên, (n-1) ký tự trội khác và ký tự thiểu số thứ hai sẽ cho một chu kỳ hợp lệ. Nếu hai ký hiệu thiểu số bằng nhau thì việc xây dựng là không thể đối với mọi độ dài ngoại trừ bốn. Đối với chiều dài bốn,`aabb`là hợp lệ. 

Cuối cùng, nếu (m>L-2), có nhiều nhất một hoặc hai vị trí bị các ký tự khác chiếm giữ. Điều đó để lại quá nhiều cửa sổ có độ dài-(n) bao gồm cùng một ký tự chi phối, do đó, việc lặp lại cửa sổ là không thể tránh khỏi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(L!L^2)) | (O(L)) | Quá chậm | 
| Tối ưu | (O(L)) | (O(L)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gọi (L) là độ dài đầu vào và đếm số lần xuất hiện của tất cả 26 chữ cái. Tìm ký tự (a) có tần số lớn nhất (m). Chúng ta chỉ cần phân bố tần số vì vị trí ban đầu không có ý nghĩa gì sau khi xáo trộn. 
2. Nếu (L=2), chấp nhận chính xác khi hai ký tự khác nhau. Mỗi cửa sổ tuần hoàn có độ dài bằng một, do đó hai vị trí phải chứa các chữ cái khác nhau. 
3. Với (L\geq3), bác bỏ ngay khi (m>L-2). Có quá ít ký tự không phải (a) để ngắt quãng đủ dài (a), do đó, một số cửa sổ tuần hoàn có độ dài-(n) phải lặp lại. 
4. Nếu (m=L-2), kiểm tra hai ký tự không phải (a). Nếu chúng giống nhau thì bác bỏ trừ khi (L=4). Đối với (L=4), multiset chính xác là`{a,a,b,b}`, Và`aabb`hoạt động. 
5. Nếu (m=L-2) và hai ký tự thiểu số khác nhau, hãy xây dựng 
(a^{n-1}ba^{n-1}c). 
Mỗi ký tự thiểu số ngăn cách một khối dài`a`các ký tự và hai dấu phân cách khác nhau nên các cửa sổ tuần hoàn xung quanh hai ranh giới không thể trùng nhau. 
6. Nếu (n<m<L-2), chọn bất kỳ ký tự nào (b\neq a), đặt (n-1) bản sao của (a), sau đó một (b), sau đó là tất cả các bản sao còn lại của (a), và cuối cùng là tất cả các ký tự không phải (a) còn lại. Hình thức kết quả là 
(a^{n-1}ba^{m-n+1}R), 
trong đó (R) chứa mọi ký tự không phải (a) còn lại. 

Khối đầu tiên chứa chính xác (n-1) bản sao của ký tự chi phối, do đó không có cửa sổ độ dài-(n) nào có thể tồn tại hoàn toàn bên trong khối đó. (b) được chèn sẽ phân tách hai khối ưu thế, trong khi tất cả các ký tự không phải (a) khác được hoãn lại ở khối cuối cùng. Điều này mang lại cho mỗi cửa sổ tuần hoàn một vị trí duy nhất so với cấu trúc phân cách. 
7. Nếu (m\leq n), xuất ra các chữ cái theo thứ tự sắp xếp. Lần chạy tối đa có độ dài tối đa là (n) và cách sắp xếp tuần hoàn được sắp xếp có mẫu ranh giới lần chạy khác nhau ở mỗi vị trí bắt đầu. Do đó, các cửa sổ có chiều dài (n) của nó được phân biệt theo từng cặp. 

### Tại sao nó hoạt động 

Điều bất biến đằng sau tất cả các cấu trúc là hai cửa sổ tuần hoàn bằng nhau sẽ phải gặp chính xác cùng một chuỗi ký tự chạy theo cùng một thứ tự. Trong trường hợp được sắp xếp, một cửa sổ lặp lại sẽ yêu cầu thời gian chạy dài hơn (n), mâu thuẫn với (m\leq n). Trong cấu trúc ký tự nặng, ký tự trội được phân chia sao cho mỗi cửa sổ có độ dài-(n) có mối quan hệ duy nhất với dấu phân cách được chèn và khối không trội còn lại. Ở mức cực đoan (m=L-2), cần có hai đặc điểm thiểu số khác nhau để phân biệt hai ranh giới. Khi (m>L-2), không có đủ dải phân cách để ngăn các cửa sổ chiếm ưu thế lặp lại, điều này chứng tỏ là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_string(s: str):
    L = len(s)

    cnt = [0] * 26
    for ch in s:
        cnt[ord(ch) - 97] += 1

    if L == 2:
        if cnt[0] == 1 and cnt[1] == 1:
            return "YES", "ab"
        # More generally, any two different letters work.
        if max(cnt) == 1:
            letters = [chr(i + 97) for i, x in enumerate(cnt) if x]
            return "YES", "".join(letters)
        return "NO", ""

    mx = max(cnt)
    w = cnt.index(mx)
    a = chr(w + 97)

    if mx > L - 2:
        return "NO", ""

    others = [i for i in range(26) if cnt[i] and i != w]

    if mx == L - 2:
        if len(others) == 1:
            if L == 4:
                b = chr(others[0] + 97)
                return "YES", a * mx + b * cnt[others[0]]
            return "NO", ""

        b = chr(others[0] + 97)
        c = chr(others[1] + 97)
        half = L // 2
        ans = a * (half - 1) + b + a * (half - 1) + c
        return "YES", ans

    if mx > L // 2:
        b_idx = others[0]
        b = chr(b_idx + 97)

        first_a = L // 2 - 1
        remaining_a = mx - first_a

        cnt[b_idx] -= 1

        tail = []
        for i in range(26):
            if cnt[i]:
                tail.append(chr(i + 97) * cnt[i])

        ans = a * first_a + b + a * remaining_a + "".join(tail)
        return "YES", ans

    # mx <= L/2
    ans = []
    for i in range(26):
        if cnt[i]:
            ans.append(chr(i + 97) * cnt[i])

    return "YES", "".join(ans)

def main():
    s = input().strip()
    ok, ans = solve_string(s)

    if ok == "NO":
        print("NO")
    else:
        print("YES")
        print(ans)

if __name__ == "__main__":
    main()
```Phần đầu tiên đếm các chữ cái trong một lần quét tuyến tính. Vì chỉ có 26 chữ cái có thể có nên việc tìm tần số tối đa và thu thập các ký tự khác đòi hỏi công việc bổ sung liên tục sau khi quét. 

Trường hợp (L=2) được xử lý riêng vì các quy tắc biên chung (L-2) được viết cho (L\geq3). Với hai vị trí, các cửa sổ bắt buộc có độ dài bằng một, do đó sự bằng nhau của hai chữ cái ngay lập tức xác định câu trả lời. 

các`mx > L - 2`kiểm tra là điều kiện không thể thực hiện được trên toàn cầu. Nó phải xảy ra trước các nhánh xây dựng vì các nhánh sau đảm nhận ít nhất hai vị trí không chiếm ưu thế tồn tại khi chúng cần dải phân cách. 

Khi`mx == L - 2`, số ký tự còn lại chính xác là hai.`others`do đó có một phần tử, nghĩa là cả hai vị trí còn lại đều chứa cùng một ký tự hoặc hai phần tử, nghĩa là chúng khác nhau. Ngoại lệ có độ dài bốn chính xác là hợp lệ`aabb`trường hợp. 

Vì`mx > L // 2`, khối ưu thế đầu tiên có`L // 2 - 1`nhân vật. Một bản sao của ký tự khác bị loại bỏ làm dấu phân cách và các bản sao chính còn lại tạo thành khối ưu thế thứ hai. Sau đó, mảng tần số còn lại được sử dụng để nối thêm tất cả các ký tự chưa được xử lý. Việc giảm số lượng dấu phân cách trước khi tạo đuôi là điều cần thiết, nếu không một bản sao sẽ được in hai lần. 

Nhánh cuối cùng chỉ đơn giản là cấu trúc được sắp xếp. Phép nhân chuỗi Python rất hữu ích ở đây vì nó xây dựng trực tiếp các khối lớn lặp lại và kích thước đầu vào (10^6) đủ lớn để việc nối thêm nhiều lần các ký tự riêng lẻ sẽ tốn kém một cách không cần thiết. 

Không cần băm chuỗi con hoặc so sánh cửa sổ rõ ràng. Bản thân việc xây dựng cung cấp thuộc tính cần thiết, do đó việc triển khai chỉ phải tái tạo nhiều tập hợp đầu vào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là`cbbabcacbb`, có độ dài là (10), do đó độ dài cửa sổ yêu cầu là (5). Tần số của nó là (a=2), (b=5) và (c=3). Tần số tối đa chính xác là (5=L/2), do đó áp dụng cấu trúc được sắp xếp. 

| Bước | (L) | (n=L/2) | tần số tối đa | Chi nhánh | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Đếm chữ cái | 10 | 5 | 5 | Đếm | (a^2b^5c^3) | 
| Kiểm tra (m>L-2) | 10 | 5 | 5 | Sai | Tiếp tục | 
| Kiểm tra (m=L-2) | 10 | 5 | 5 | Sai | Tiếp tục | 
| Kiểm tra (m>L/2) | 10 | 5 | 5 | Sai | Trường hợp được sắp xếp | 
| Xây dựng | 10 | 5 | 5 | Đã sắp xếp |`aabbbbbccc`| 

Chu kỳ kết quả là`aabbbbbccc`. Mười cửa sổ dài năm chu kỳ của nó là`aabbb`,`abbbb`,`bbbbb`,`bbbbc`,`bbbcc`,`bbccc`,`bccca`,`cccaa`,`ccaab`, Và`caabb`. 

Mỗi người đều khác nhau. Mẫu chính thức sử dụng một cách sắp xếp hợp lệ khác, điều này được cho phép vì bài toán chấp nhận bất kỳ sự khôi phục nào thỏa mãn điều kiện. 

### Mẫu 2 

Đầu vào là`aa`, vì vậy (L=2). Cả hai ký tự đều bằng nhau, nghĩa là cả hai cửa sổ tuần hoàn có độ dài bằng một`a`. 

| Bước | (L) | tần số tối đa | Chi nhánh | Kết quả | 
| --- | --- | --- | --- | --- | 
| Đếm chữ cái | 2 | 2 | Chiều dài tối thiểu |`a`xảy ra hai lần | 
| Kiểm tra sự bình đẳng | 2 | 2 | Ký hiệu bằng nhau |`NO`| 

Không thể sắp xếp lại được vì việc sắp xếp lại hai ký hiệu bằng nhau không làm thay đổi gì cả. Đây chính xác là trường hợp không thể xảy ra ở mẫu thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(L)) | Đầu vào được quét một lần và tối đa (L) ký tự được ghi vào câu trả lời. | 
| Không gian | (O(L)) | Bản thân chuỗi đầu ra có độ dài (L), trong khi mảng tần số có kích thước không đổi 26. | 

Với (L\leq10^6), xử lý tuyến tính phù hợp với giới hạn một giây, trong khi các phương pháp tiếp cận giai thừa hoặc bậc hai vượt xa ngân sách sẵn có. Thuật toán chỉ thực hiện một vài lần chuyển qua đầu vào và xây dựng câu trả lời một cách trực tiếp. 

## Trường hợp thử nghiệm```
# helper: run solution on input string, return output string
def run(inp: str) -> str:
    s = inp.strip()
    ok, ans = solve_string(s)
    if ok == "NO":
        return "NO\n"
    return "YES\n" + ans + "\n"

def valid_cycle(original: str, output: str) -> bool:
    lines = output.strip().splitlines()
    if lines[0] == "NO":
        return False

    ans = lines[1]
    if len(ans) != len(original):
        return False

    if sorted(ans) != sorted(original):
        return False

    L = len(ans)
    n = L // 2

    windows = set()
    for i in range(L):
        w = "".join(ans[(i + j) % L] for j in range(n))
        if w in windows:
            return False
        windows.add(w)

    return len(windows) == L

# Provided sample 1.
out = run("cbbabcacbb")
assert valid_cycle("cbbabcacbb", out), "sample 1"

# Provided sample 2.
assert run("aa") == "NO\n", "sample 2"

# Provided sample 3.
out = run("afedbc")
assert valid_cycle("afedbc", out), "sample 3"

# Minimum-size input.
assert run("ab") == "YES\nab\n", "minimum size"

# All characters equal.
assert run("aaaa") == "NO\n", "all equal"

# L = 6, max frequency = L - 2, with two different minority characters.
out = run("aaaabc")
assert out == "YES\naabaac\n", "two different minority characters"

# Heavy majority, but not at the impossible boundary.
out = run("aaaaabbc")
assert out == "YES\naaabaabc\n", "heavy majority construction"

# Maximum-size input, max frequency exactly L/2.
large = "a" * 500_000 + "b" * 500_000
out = run(large)
assert out.startswith("YES\n"), "maximum size"
assert out[4:].strip() == large, "maximum size sorted construction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ab`|`YES`Và`ab`| Độ dài tối thiểu và cửa sổ một ký tự riêng biệt | 
|`aaaa`|`NO`| Ranh giới hoàn toàn bằng nhau và các cửa sổ lặp lại không thể tránh khỏi | 
|`aaaabc`|`YES`,`aabaac`| (m=L-2) với hai nhân vật thiểu số khác nhau | 
|`aaaaabbc`|`YES`,`aaabaabc`| Công trình xây dựng đa số nặng với (L/2<m<L-2) | 
| 500.000`a`ký tự theo sau là 500.000`b`nhân vật |`YES`và chuỗi được sắp xếp | Kích thước đầu vào tối đa và ranh giới (m=L/2) | 

## Vỏ cạnh 

cho`aa`, thuật toán đi vào nhánh có độ dài hai chuyên dụng. Tần số tối đa là hai, do đó, cả hai cửa sổ có độ dài một được yêu cầu sẽ là`a`. Nó in`NO`, phù hợp với kết quả duy nhất có thể xảy ra. 

Vì`aaaa`, chiều dài là bốn và (m=4>L-2=2). Việc kiểm tra bất khả thi sẽ xảy ra ngay lập tức. Không có đủ không`a`các ký tự để tách chu trình thành hai cửa sổ có độ dài riêng biệt, do đó không cần cố gắng xây dựng sau này. 

Vì`aabb`, chiều dài là bốn và (m=2=L/2=L-2). Hai vị trí thiểu số có cùng một đặc tính, điều này thường làm cho trường hợp (L-2) không thể thực hiện được. Ngoại lệ đặc biệt có độ dài bốn chấp nhận nó và tạo ra`aabb`. Cửa sổ tuần hoàn của nó là`aa`,`ab`,`bb`, Và`ba`. 

Vì`aaaabb`, chiều dài là sáu và (m=4=L-2), nhưng cả hai vị trí thiểu số đều chứa`b`. Vì độ dài không phải là bốn nên thuật toán sẽ in`NO`. Vấn đề không chỉ đơn thuần là việc sắp xếp thất bại. Bất kỳ sự sắp xếp nào cũng không có đủ dấu phân cách để tạo ra sáu cửa sổ có chiều dài ba vòng khác nhau. 

Vì`aaaabc`, ký tự trội xuất hiện bốn lần, chính xác là (L-2), còn hai ký tự còn lại thì khác nhau. Cấu trúc là (a^{2}ba^{2}c), cho`aabaac`. Sáu cửa sổ dài ba chu kỳ của nó là`aab`,`aba`,`baa`,`aac`,`aca`, Và`caa`, vì vậy mỗi cửa sổ là duy nhất. 

Vì`aaaaabbc`, ký tự trội xuất hiện năm lần trong một chuỗi có độ dài tám. Ở đây (n=4) và (n<m<L-2), do đó cấu trúc đa số nặng được sử dụng. Nó tạo ra`aaa`+`b`+`aa`+`bc`, cụ thể là`aaabaabc`. Ký tự nổi bật được phân chia xung quanh dấu phân cách, ngăn các cửa sổ lặp lại xuất hiện trong cách sắp xếp. 

Đối với trường hợp có kích thước tối đa 500.000`a`ký tự và 500.000`b`ký tự, tần số tối đa là chính xác (L/2). Thuật toán rơi vào nhánh được sắp xếp và đưa ra cùng một chuỗi được sắp xếp. Đầu vào lớn được xử lý bằng cách đếm tần số tuyến tính và xây dựng chuỗi trực tiếp, do đó kích thước của nó không làm thay đổi hành vi tiệm cận.
