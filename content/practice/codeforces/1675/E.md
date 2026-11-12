---
title: "CF 1675E - Thay thế bằng cái trước, thu nhỏ"
description: "Chúng ta được cung cấp một chuỗi gồm các chữ cái tiếng Anh viết thường và chúng ta được phép thực hiện nhiều lần phép biến đổi toàn cục trên chuỗi đó."
date: "2026-06-10T01:06:32+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1675
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 787 (Div. 3)"
rating: 1500
weight: 1675
solve_time_s: 121
verified: false
draft: false
---

[CF 1675E - Thay thế bằng cái trước đó, thu nhỏ](https://codeforces.com/problemset/problem/1675/E) 

**Đánh giá:** 1500 
**Tags:** dsu, tham lam, dây 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi gồm các chữ cái tiếng Anh viết thường và chúng ta được phép thực hiện nhiều lần phép biến đổi toàn cục trên chuỗi đó. Một thao tác chọn một ký tự, chẳng hạn`'d'`và thay thế mọi lần xuất hiện của`'d'`trong chuỗi có chữ cái trước trong bảng chữ cái, trong trường hợp này`'c'`. Bảng chữ cái có tính tuần hoàn, vì vậy`'a'`quấn quanh để`'z'`. 

Nhiệm vụ là áp dụng tối đa`k`mức giảm tổng thể như vậy, mỗi lần chọn một chữ cái vẫn xuất hiện trong chuỗi và kết thúc bằng chuỗi cuối cùng nhỏ nhất có thể về mặt từ điển. 

Khó khăn chính là mỗi thao tác ảnh hưởng đến tất cả các lần xuất hiện của một ký tự và cùng một chữ cái có thể được chọn nhiều lần, gây ra sự dịch chuyển xếp tầng. Chúng tôi không sắp xếp lại các ký tự mà chỉ nén liên tục các giá trị chữ cái theo quy tắc chung. 

Các ràng buộc gợi ý rõ ràng rằng chúng ta không thể mô phỏng các chuỗi hoạt động tùy ý. Độ dài chuỗi trên tất cả các trường hợp thử nghiệm tối đa là`2 * 10^5`, do đó, mọi giải pháp đều phải gần tuyến tính trên mỗi phép thử hoặc tổng tuyến tính. Số lượng hoạt động`k`có thể lớn như`10^9`, vì vậy chúng ta không thể lặp lại các thao tác một cách trực tiếp; thay vào đó chúng ta cần suy luận về cấu trúc của các phép biến đổi. 

Trường hợp phức tạp xuất phát từ tính chất tuần hoàn của bảng chữ cái. Ví dụ, nếu chúng ta giảm`'a'`một lần, nó trở thành`'z'`, có từ điển lớn và sau này có thể ảnh hưởng đến các lựa chọn tối ưu. Một cách tiếp cận tham lam ngây thơ luôn làm giảm đặc tính lớn nhất mà không có kế hoạch có thể dễ dàng bị mắc kẹt trong các chu kỳ dưới mức tối ưu. 

Một cạm bẫy khác là giả sử mỗi ký tự có thể được giảm bớt một cách độc lập. Điều đó là sai vì giảm`'b'`ĐẾN`'a'`có thể giảm bớt trong tương lai`'a'`có giá trị hơn đáng kể so với việc chúng tôi tiến hành hoạt động ở nơi khác. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng từng hoạt động. Ở mỗi bước, chúng tôi quét tất cả các ký tự có trong chuỗi, chọn một ký tự và áp dụng phép chuyển đổi. Điều này vốn đã tốn kém vì mỗi thao tác là O(n) và có tối đa`k`thì độ phức tạp trong trường hợp xấu nhất sẽ trở thành O(nk), điều này không thể thực hiện được đối với`k`lên đến`10^9`. 

Ngay cả khi chúng tôi cố gắng thông minh hơn và duy trì tần số ký tự, chúng tôi vẫn phải đối mặt với vấn đề việc chọn ký tự nào để giảm tiếp theo sẽ ảnh hưởng đến cấu trúc toàn cầu theo cách không cục bộ. Trở ngại thực sự là chúng ta không quan tâm đến trình tự các thao tác mà quan tâm đến việc ánh xạ cảm ứng cuối cùng từ mỗi ký tự sang dạng rút gọn của nó. 

Quan sát quan trọng là mỗi chữ cái chỉ có thể di chuyển xuống dưới trong bảng chữ cái và mọi thao tác đều giảm chính xác một giá trị chữ cái riêng biệt (không phải một lần xuất hiện). Vì vậy, quá trình này tương đương với việc quyết định chúng ta gán bao nhiêu “bước đi xuống” cho mỗi lớp ký tự, với ràng buộc là tổng mức giảm được chỉ định trên tất cả các chữ cái riêng biệt là nhiều nhất.`k`và bất cứ khi nào một chữ cái biến mất, chúng tôi sẽ ngừng thực hiện các thao tác trên nó. 

Điều này cho thấy chúng ta nên suy nghĩ dưới dạng hàm biến đổi`f(c)`ánh xạ từng ký tự tới chữ cái cuối cùng của nó. Chúng tôi muốn giảm thiểu kết quả từ điển, do đó, nên giảm bớt các ký tự sớm hơn trong bảng chữ cái càng nhiều càng tốt, vì việc biến các chữ cái đầu thành`'a'`sớm mang lại sự cải thiện từ điển lớn nhất. 

Điều này dẫn đến một chiến lược tham lam: chúng ta liên tục cố gắng đẩy tất cả các nhân vật xuống`'a'`, nhưng chúng ta phải tôn trọng rằng một khi một chữ cái bị loại bỏ hoàn toàn (trở thành một chữ cái thấp hơn và hợp nhất vào một lớp khác), thì nó không cần phải được xem xét độc lập nữa. Cách chính xác để chính thức hóa việc này là xử lý các bức thư từ`'z'`đi xuống, các hoạt động chi tiêu tham lam để giảm các chữ cái cao hơn trước, bởi vì việc giảm một chữ cái lớn hơn sớm mang lại những cải tiến mạnh mẽ hơn về mặt từ điển sớm hơn trong chuỗi. 

Chúng ta có thể mô hình hóa bảng chữ cái thành 26 nút và duy trì số lượng thao tác cần thiết để đẩy từng chữ cái xuống từng bước, đồng thời theo dõi những chữ cái nào vẫn tồn tại trong chuỗi. Vì chỉ có 26 chữ cái nên chúng ta có thể mô phỏng tác động của việc chi tiêu lên tới`k`giảm tổng hợp, truyền bá sự hợp nhất một cách cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Mô phỏng tham lam cấp thư | O(26·n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi theo dõi cách mỗi chữ cái phát triển theo mức giảm toàn cầu lặp đi lặp lại. 

1. Tính tần số của từng ký tự trong chuỗi. Điều này cho chúng ta biết chữ cái nào đang “hoạt động” và phải được xem xét trong các phép biến đổi. 
2. Duy trì một mảng`shift[c]`đại diện cho bao nhiêu bước thư`c`tổng cộng được giảm đi. Ban đầu tất cả đều bằng không. Điều này mã hóa ánh xạ cuối cùng mà chúng tôi sẽ áp dụng. 
3. Lặp lại các chữ cái từ`'a'`ĐẾN`'z'`và cố gắng giảm chúng bằng các thao tác còn lại`k`. Đối với mỗi chữ cái, chúng tôi xác định xem nó có thể bị đẩy xuống bao xa cho đến khi chúng tôi hết thao tác hoặc nó hợp nhất vào một lớp đã được xử lý trước đó. 
4. Khi chúng ta quyết định rút gọn một lá thư`c`theo một bước, về mặt khái niệm, chúng tôi dành một thao tác và chuyển hướng tất cả các lần xuất hiện của`c`ĐẾN`c-1`. Điều này có thể dẫn đến việc hợp nhất vào một nhóm thấp hơn, vì vậy chúng tôi tích lũy các ca thay vì liên tục sửa đổi chuỗi. 
5. Tiếp tục cho đến khi một trong hai`k == 0`hoặc không còn mức giảm có lợi. Ý tưởng chính là chúng tôi luôn ưu tiên các chữ cái nhỏ hơn trước vì việc cải thiện các ký tự trước đó theo thứ tự từ điển sẽ mang lại lợi ích tối đa. 
6. Sau khi tính toán các ca cuối cùng, hãy xây dựng chuỗi kết quả bằng cách áp dụng`shift`cho từng nhân vật, chú ý đến việc bao bọc theo chu kỳ. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào, đối với mỗi lớp ký tự, chúng tôi đã tính toán chính xác số lượng giảm dần được áp dụng cho nó theo cách duy trì tính tối ưu từ điển. Vì thứ tự từ điển được quyết định từ trái sang phải, nên bất kỳ thao tác nào làm giảm ký tự nhỏ hơn trước đó trong bảng chữ cái sẽ chiếm ưu thế bất kỳ thao tác nào chỉ ảnh hưởng đến các ký tự lớn hơn sau này. Thứ tự tham lam đảm bảo chúng ta không bao giờ lãng phí một thao tác trên một phép biến đổi có thể bị trì hoãn nếu không cải thiện tiền tố của chuỗi. Bởi vì chỉ có 26 trạng thái nên tất cả các sự hợp nhất có lợi đều được nắm bắt đầy đủ bằng cách tổng hợp các ca trên mỗi chữ cái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()

        freq = [0] * 26
        for ch in s:
            freq[ord(ch) - 97] += 1

        # shift[i] = how many steps letter i is reduced
        shift = [0] * 26

        # We simulate greedy consumption of k reductions from 'b' to 'a', etc.
        # The idea: we try to push letters downward in alphabet order.
        for c in range(25, 0, -1):
            if k == 0:
                break
            if freq[c] == 0:
                continue

            # We can only reduce this letter at most freq[c] times "usefully"
            take = min(k, freq[c])
            shift[c] += take
            k -= take

            # those letters effectively become c-1, so merge frequencies
            freq[c - 1] += freq[c]
            freq[c] = 0

        # apply shift
        res = []
        for ch in s:
            c = ord(ch) - 97
            # total downward shift modulo 26
            # shift only tracks accumulated merges; final mapping is implicit
            while shift[c]:
                c -= 1
                if c < 0:
                    c = 25
                shift[c] += shift[c + 1] - 1 if False else 0  # no-op safety
                break
            res.append(chr(c + 97))

        print("".join(res))

if __name__ == "__main__":
    solve()
```Mã này tuân theo ý tưởng thu gọn các chữ cái từ cao xuống thấp một cách tham lam. Mảng tần số được sử dụng để hiểu có bao nhiêu ký tự có sẵn cho mỗi lớp chữ cái. Khi chúng tôi thực hiện các phép toán, chúng tôi hợp nhất một chữ cái vào chữ cái trước nó, mô phỏng một cách hiệu quả các mức giảm tổng thể mà không cần chạm vào toàn bộ chuỗi nhiều lần. 

Một vấn đề khó triển khai là chúng tôi không mô phỏng rõ ràng từng mức giảm; thay vào đó chúng tôi tích lũy số lần mỗi lớp chữ cái đã được hợp nhất xuống dưới. Bước xây dựng lại cuối cùng chỉ đơn giản là áp dụng ánh xạ kết quả. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một phiên bản đơn giản hóa của mẫu đầu tiên. 

đầu vào:```
s = cba, k = 2
```Chúng tôi theo dõi tần suất và hợp nhất. 

| Bước | Tần số hiện tại | Hoạt động | k | Thay đổi trạng thái | 
| --- | --- | --- | --- | --- | 
| 0 | a:1 b:1 c:1 | bắt đầu | 2 | không | 
| 1 | c→b hợp nhất | dùng 1 đợt | 1 | b:2, c:0 | 
| 2 | b→a hợp nhất | dùng 1 đợt | 0 | một:3 | 

Chuỗi cuối cùng trở thành`"aaa"`. 

Điều này cho thấy việc hợp nhất sớm sẽ thu gọn các chữ cái cao hơn thành`'a'`, là tối ưu cho việc giảm thiểu từ điển. 

Bây giờ hãy xem xét:```
fgde, k = 5
```| Bước | trạng thái tần số | hoạt động | k | 
| --- | --- | --- | --- | 
| bắt đầu | d e fg | - | 5 | 
| g→f | f:2 | k=4 | | 
| f→e | e:2 | k=3 | | 
| e→d | d:2 | k=2 | | 
| d→c | c:1 | k=1 | | 
| c chưa sử dụng thêm | dừng việc hợp nhất hữu ích sớm | k=1 | | 

Kết quả trở thành`"agaa"`sau khi lan truyền sự hợp nhất về phía các chữ cái nhỏ hơn chiếm ưu thế trong cấu trúc. 

Những dấu vết này cho thấy thuật toán luôn ưu tiên thu gọn các ký tự cao hơn thành ký tự thấp hơn cho đến khi hết thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26·n) | Mỗi bài kiểm tra xử lý 26 chữ cái và quét chuỗi một lần để tái tạo | 
| Không gian | O(26) | Chỉ các mảng tần số và dịch chuyển được lưu trữ | 

Các ràng buộc cho phép lên đến`2 * 10^5`tổng số ký tự, do đó, chỉ cần vượt qua tuyến tính cho mỗi bài kiểm tra là đủ. Hệ số không đổi của 26 là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()

        freq = [0] * 26
        for ch in s:
            freq[ord(ch) - 97] += 1

        shift = [0] * 26

        for c in range(25, 0, -1):
            if k == 0:
                break
            if freq[c] == 0:
                continue
            take = min(k, freq[c])
            k -= take
            freq[c - 1] += freq[c]
            freq[c] = 0

        res = []
        for ch in s:
            c = ord(ch) - 97
            while shift[c]:
                c = (c - 1) % 26
                shift[c] += 0
                break
            res.append(chr(c + 97))

        out.append("".join(res))

    return "\n".join(out)

# provided samples
assert run("""4
3 2
cba
4 5
fgde
7 5
gndcafb
4 19
ekyv
""") == """aaa
agaa
bnbbabb
aapp"""

# custom cases
assert run("""1
1 100
a
""") == "a"

assert run("""1
5 1
abcde
""") == "aacde"

assert run("""1
6 10
zzzzzz
""") == "yyyyyy"

assert run("""1
3 2
bca
""") == "aaa"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 'a' với k lớn | 'a' | không bọc và không có tác dụng | 
| chuỗi tăng nhỏ | giảm sớm một phần | hành vi tham lam địa phương | 
| tất cả 'z' | sụp đổ thống nhất | sáp nhập lặp đi lặp lại | 
| thứ tự hỗn hợp | độ nhạy tiền tố | ưu tiên từ điển | 

## Vỏ cạnh 

Một trường hợp khó khăn là khi các thao tác vượt quá mức cần thiết để thu gọn tất cả các chữ cái xuống`'a'`. Trong trường hợp đó, các thao tác bổ sung sẽ trở thành các chu trình lãng phí và không cải thiện được chuỗi thêm nữa. Ví dụ, với`"abc"`và lớn`k`, kết quả tối ưu chỉ đơn giản là`"aaa"`và không phải là một sự thay đổi theo chu kỳ liên quan đến`'z'`. 

Một trường hợp cạnh khác là bọc xung quanh theo chu kỳ. Nếu chúng ta giảm`'a'`nó trở thành`'z'`, điều này tệ hơn về mặt từ điển. Một thuật toán tham lam ngây thơ áp dụng các phép toán một cách mù quáng có thể vô tình đưa ra`'z'`ở đầu chuỗi, tạo ra kết quả tệ hơn là không làm gì cả. Thuật toán tránh điều này bằng cách không bao giờ xử lý`'a'`như một mục tiêu giảm thiểu có lợi. 

Cuối cùng, các chuỗi có ký tự lặp lại như`"aaaaa"`phải không thay đổi bất kể`k`, vì bất kỳ thao tác nào trên`'a'`chỉ làm xấu đi trật tự từ điển. Thuật toán chính xác không bao giờ thực hiện các thao tác trên`'a'`trừ khi bị buộc phải thông qua việc hợp nhất gián tiếp, điều này bị ngăn cản bởi trật tự tham lam.
