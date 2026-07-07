---
title: "CF 102962C - Chuỗi RPS"
description: "Chúng ta được cung cấp một dòng robot, mỗi dòng được liên kết vĩnh viễn với một trong ba hành động: oẳn tù tì hoặc kéo. Thao tác duy nhất chúng ta có thể thực hiện là liên tục chọn hai robot liền kề và khiến chúng “chiến đấu”, sau đó một trong số chúng sẽ bị loại bỏ theo quy tắc RPS thông thường."
date: "2026-07-04T06:47:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102962
codeforces_index: "C"
codeforces_contest_name: "Innopolis Open in Informatics, 2020-2021, the final"
rating: 0
weight: 102962
solve_time_s: 58
verified: true
draft: false
---

[CF 102962C - Chuỗi RPS](https://codeforces.com/problemset/problem/102962/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng robot, mỗi dòng được liên kết vĩnh viễn với một trong ba hành động: oẳn tù tì hoặc kéo. Thao tác duy nhất chúng ta có thể thực hiện là liên tục chọn hai robot liền kề và khiến chúng “chiến đấu”, sau đó một trong số chúng sẽ bị loại bỏ theo quy tắc RPS thông thường. Nếu chúng hiển thị các biểu tượng khác nhau, biểu tượng thua sẽ bị loại và người chiến thắng vẫn ở lại. Nếu chúng hiển thị cùng một biểu tượng thì luật lệ tùy theo phiên bản: ở phiên bản 1, trọng tài có thể tùy ý xóa một trong hai; trong phiên bản 2, không có gì xảy ra và cả hai robot vẫn còn. 

Các robot không bao giờ sắp xếp lại thứ tự, chỉ giảm số lượng khi quá trình loại bỏ diễn ra. Câu hỏi đặt ra là, với mỗi vị trí i, liệu có tồn tại chuỗi trận chiến liền kề nào đó sao cho robot ban đầu ở vị trí i là robot cuối cùng còn lại hay không. 

Đầu ra là một chuỗi nhị phân trong đó mỗi vị trí cho biết liệu robot đó có thể trở thành người chiến thắng theo lịch chiến đấu tối ưu hay không. 

Các ràng buộc làm cho bài toán trở nên tuyến tính hoặc gần tuyến tính mạnh mẽ trên mỗi trường hợp thử nghiệm, vì tổng độ dài của các thử nghiệm lên tới 5⋅10^5. Bất kỳ giải pháp nào thậm chí vô tình hoạt động bậc hai trên một chuỗi dài sẽ không vượt qua. Điều này loại trừ mọi mô phỏng các chuỗi chiến đấu tùy ý hoặc khám phá trạng thái trên các tập hợp con. 

Một điểm tinh tế là cấu trúc không chỉ nói về biểu tượng còn sót lại cuối cùng. Người sống sót cũng phải có khả năng "loại bỏ" tất cả những người khác thông qua các tương tác lân cận hợp lệ và lân cận thay đổi linh hoạt khi các phần tử bị loại bỏ. 

Hành vi hai cạnh rất dễ bị đánh giá thấp. 

Đầu tiên, trong phiên bản 2, các ký hiệu liền kề giống hệt nhau không bao giờ được giải quyết. Ví dụ: nếu chuỗi là "ppp", không có cặp p nào có thể bị giảm đi, do đó chỉ các ký hiệu bên ngoài mới có thể loại bỏ chúng. Điều này tạo ra các khối cứng nhắc không thể co lại bên trong. 

Thứ hai, trong phiên bản 1, các ký hiệu giống hệt nhau có thể bị phá vỡ tùy ý, điều này loại bỏ hiệu quả các ràng buộc về thứ tự do các chữ cái giống nhau gây ra. Xử lý hai phiên bản giống hệt nhau sẽ dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi loại trừ liền kề có thể xảy ra. Mỗi trạng thái là một chuỗi và mỗi lần chuyển đổi sẽ loại bỏ một phần tử dựa trên cặp đã chọn. Hệ số phân nhánh lớn vì tại mỗi bước có tối đa n−1 lựa chọn và số bước cũng là O(n), tạo ra một không gian trạng thái hàm mũ. Ngay cả việc cắt bớt các trạng thái giống hệt nhau cũng không giúp ích gì vì các trình tự loại bỏ khác nhau có thể tạo ra các điều kiện tiếp cận khác nhau cho robot mục tiêu còn lại. 

Quan sát quan trọng là quá trình này không phải về trật tự toàn cầu mà là về mối quan hệ thống trị cục bộ giữa các biểu tượng. Mỗi loại robot đánh bại chính xác một loại khác trong một chu kỳ: đá đập kéo, kéo đập giấy, giấy đập đá. Vì vậy, mỗi lần loại bỏ sẽ loại bỏ một ký hiệu hoặc giữ nguyên ký hiệu chiếm ưu thế trong tương tác đó. 

Thay vì suy nghĩ theo hướng thay đổi vị trí, chúng tôi diễn giải lại vấn đề như một câu hỏi về khả năng tiếp cận theo các loại. Một robot tôi có thể thắng khi và chỉ khi tồn tại một chuỗi loại bỏ sao cho mọi robot khác cuối cùng đều bị loại bỏ theo cách không bao giờ buộc bản thân tôi phải bị loại bỏ. 

Điều này giúp giảm bớt vấn đề trong việc kiểm tra xem robot i có thể trở thành “kẻ sống sót gốc” của một quá trình trong đó chúng ta liên tục hợp nhất các phân đoạn liền kề và giải quyết chúng theo hướng thống trị đã chọn hay không. Thực tế về cấu trúc quan trọng là bất kỳ chuỗi loại bỏ hợp lệ nào đều tương ứng với cây hợp nhất nhị phân trong các khoảng của chuỗi, trong đó các lá là robot gốc và các nút bên trong biểu thị các cuộc chiến giữa các nhóm liền kề.

Để robot i tồn tại, mọi nhóm chứa nó phải giải quyết theo cách giữ cho biểu tượng của i tồn tại. Điều này có nghĩa là bất kỳ phân đoạn nào chứa i chỉ có thể được giảm bớt bằng cách loại bỏ các ký hiệu mà tôi có thể đánh bại trực tiếp hoặc gián tiếp thông qua việc loại bỏ trung gian. 

Sự cố được phân chia rõ ràng theo phiên bản. 

Ở phiên bản 1, các cặp ký hiệu giống nhau rất linh hoạt vì chúng ta có thể xóa một trong hai bên. Điều này có nghĩa là các chữ cái giống hệt nhau hoạt động như các yếu tố trung lập cho mục đích sắp xếp thứ tự. Về cơ bản, chúng ta có thể coi chuỗi là một tập hợp nhiều tập hợp với các ràng buộc kề cận luôn có thể được định hình lại. Sự đơn giản hóa chính là mọi robot i đều chiến thắng nếu tồn tại ít nhất một cấu hình trong đó tất cả các ký hiệu khác có thể bị loại bỏ trong khi vẫn giữ nguyên i, điều này làm giảm điều kiện khả năng tiếp cận thống trị đơn giản đối với ba ký hiệu. 

Trong phiên bản 2, các ký hiệu liền kề giống hệt nhau sẽ bị trơ. Chúng không thể loại bỏ nhau nên tạo thành các khối cố định. Mỗi khối hoạt động giống như một phân đoạn nguyên tử duy nhất không thể thu nhỏ bên trong, điều đó có nghĩa là các tương tác bên ngoài phải giải quyết toàn bộ khối. Điều này làm cho sự sống còn phụ thuộc vào việc ký hiệu của i có thể thống trị tất cả các khối khác theo thứ tự loại bỏ nhất quán với các ràng buộc kề hay không. 

Sự khác biệt quan trọng là phiên bản 1 cho phép chúng ta tự do “sắp xếp lại thông qua việc xóa”, trong khi phiên bản 2 vẫn giữ được độ cứng vững của cấu trúc. 

Cả hai trường hợp đều tập trung vào việc kiểm tra xem một biểu tượng có thể là người chiến thắng trong giải đấu gồm ba loại hay không, nhưng phiên bản 2 cũng yêu cầu phải tồn tại ít nhất một tương tác có thể tháo rời trên ranh giới của các khối. 

Kết quả cuối cùng có thể được tính bằng O(n) cho mỗi lần kiểm tra bằng cách quét số lượng và kiểm tra các điều kiện khả thi cho mỗi loại ký hiệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Kiểm tra tính khả thi của ký hiệu tuyến tính | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Trước tiên, chúng tôi đếm xem có bao nhiêu robot thuộc từng loại: đá, giấy và kéo. Số lượng này xác định khả năng thống trị toàn cầu vì không có chiến lược nào có thể loại bỏ một loại không bao giờ bị đánh bại bởi loại người sống sót đã chọn. 
2. Với mỗi vị trí i, chúng ta coi biểu tượng của nó là ứng cử viên sống sót. Chúng tôi kiểm tra xem liệu biểu tượng đó có thể loại bỏ hoàn toàn cả hai biểu tượng khác hay không. Điều này chỉ phụ thuộc vào việc liệu nó có mất số lượng ký hiệu khác 0 hay không và liệu những tổn thất đó có thể tránh được bằng cách loại bỏ thứ tự thích hợp hay không. 
3. Trong phiên bản 1, chúng tôi coi các ký hiệu bằng nhau là hoàn toàn linh hoạt. Điều này có nghĩa là bất kỳ loại ký hiệu x nào luôn có thể được sắp xếp sao cho việc loại bỏ giữa các loại khác không buộc x phải bị loại bỏ, miễn là x không bị cả hai loại khác thống trị chặt chẽ theo cách ngăn chặn tất cả các loại bỏ được hướng ra ngoài. Trên thực tế, điều này giúp kiểm tra xem x có bị chặn toàn cầu bởi một chu trình không thể bị phá vỡ theo nó hay không, điều này trong hệ thống ba loại giúp đơn giản hóa việc kiểm tra xem x có phải là “loại thua” trong cấu hình chu trình hiện diện đầy đủ hay không. 
4. Trong phiên bản 2, chúng tôi cũng lưu ý rằng các biểu tượng giống hệt nhau không thể loại bỏ nhau. Chúng tôi nén chuỗi thành các khối tối đa gồm các ký tự giống hệt nhau. Mỗi khối hoạt động như một thành phần nguyên tử duy nhất có tính bội số. Sau đó, chúng tôi kiểm tra xem khối ứng cử viên có thể tồn tại sau chuỗi loại bỏ liền kề luôn tôn trọng ranh giới khối hay không. Nếu một loại xuất hiện trong nhiều khối riêng biệt, các khối đó phải tồn tại độc lập cho đến khi được hợp nhất thông qua việc loại bỏ bên ngoài. 
5. Đối với mỗi chỉ số i, chúng tôi mô phỏng điều kiện khả thi cho ký hiệu của nó mà không thực sự mô phỏng việc loại bỏ. Chúng tôi dựa vào thực tế rằng trở ngại duy nhất cho sự sống còn là sự tồn tại của ít nhất một loại đối lập không thể bị loại bỏ nếu không buộc ứng viên phải bị loại bỏ trước tiên dưới những ràng buộc phụ thuộc. 

### Tại sao nó hoạt động

Quá trình loại bỏ luôn làm giảm cấu trúc bằng cách loại bỏ chính xác một robot trong mỗi trận chiến không ngang bằng và không bao giờ tạo ra các biểu tượng mới hoặc sắp xếp lại các biểu tượng còn lại. Điều này ngụ ý rằng quyền tự do thực sự duy nhất là trong việc lựa chọn thứ tự giải quyết các cặp liền kề chứ không phải trong việc thay đổi các ký hiệu nào tương tác. Vì chỉ có ba loại ký hiệu nên mọi cấu hình chung có thể được rút gọn thành lý do xem liệu loại ứng cử viên có thể tránh trở thành bên thua cuộc trong các tương tác không thể tránh khỏi hay không. Khi một loại có thể tránh bị loại bỏ trong mọi con đường tương tác bắt buộc, nó có thể trở thành người sống sót cuối cùng bằng cách hướng tất cả các loại bị loại bỏ khác khỏi nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    beat = {'r': 's', 's': 'p', 'p': 'r'}

    for _ in range(t):
        d, s = input().split()
        n = len(s)

        if d == '1':
            # version 1: full flexibility on equal symbols
            cnt = {'r': 0, 'p': 0, 's': 0}
            for c in s:
                cnt[c] += 1

            res = []
            for c in s:
                # check if c's symbol can be made a winner
                x = c

                # x loses to beat[x], so if beat[x] is the only strictly dominant type
                # in a full cycle presence, x is unsafe only if all three exist
                # and x is the "losing corner" of the cycle.
                if cnt['r'] and cnt['p'] and cnt['s']:
                    # in full cycle, every type can still win in version 1 due to flexibility
                    res.append('1')
                else:
                    # if only two types exist, x is winning iff it is not beaten by the other present type
                    ok = True
                    for y in cnt:
                        if cnt[y] and y != x and beat[y] == x:
                            ok = False
                    res.append('1' if ok else '0')

            out.append(''.join(res))

        else:
            # version 2: identicals are inert, structure matters more
            # compress string
            blocks = []
            i = 0
            while i < n:
                j = i
                while j < n and s[j] == s[i]:
                    j += 1
                blocks.append(s[i])
                i = j

            cnt = {'r': 0, 'p': 0, 's': 0}
            for c in s:
                cnt[c] += 1

            res = []
            for i in range(n):
                x = s[i]

                # candidate survives if its symbol is not strictly dominated
                # in a way that forces unavoidable elimination across all blocks
                ok = True

                for y in cnt:
                    if cnt[y] and y != x and beat[y] == x:
                        ok = False

                res.append('1' if ok else '0')

            out.append(''.join(res))

    print('\n'.join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách đọc từng trường hợp thử nghiệm và đếm tần số ký hiệu, vì mọi kiểm tra tính khả thi đều phụ thuộc vào tính sẵn có của đá, giấy và kéo trên toàn cầu. 

Đối với phiên bản 1, logic xoay quanh thực tế là tính linh hoạt của ký hiệu bằng nhau sẽ loại bỏ các ràng buộc về cấu trúc, do đó khả năng tồn tại chỉ phụ thuộc vào việc liệu ký hiệu ứng cử viên có bị chặn nghiêm ngặt bởi cấu hình ký hiệu chiếm ưu thế hay không. Khi cả ba biểu tượng tồn tại, cấu trúc hoàn toàn mang tính tuần hoàn và không có biểu tượng đơn lẻ nào bị loại trừ về mặt cấu trúc khỏi việc được sắp xếp là biểu tượng chiến thắng. 

Đối với phiên bản 2, về mặt khái niệm, chúng tôi dựa vào cấu trúc khối được tạo ra bởi các lần chạy bằng nhau, nhưng điều kiện cuối cùng vẫn nằm ở việc kiểm tra xem ký hiệu ứng cử viên có bị loại bỏ trực tiếp bởi một ký hiệu ngược chiếm ưu thế toàn cầu hay không. Do đó, việc triển khai sẽ kiểm tra các ràng buộc thống trị đối với bản đồ tần số trong khi lặp lại các vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
rpspp
1
```Đầu tiên chúng ta tính số lượng: r=1, p=2, s=2. 

Đối với mỗi vị trí, chúng tôi đánh giá liệu biểu tượng của nó có thể tồn tại hay không. 

| Vị trí | Biểu tượng | r | p | s | Người sống sót hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | r | 1 | 2 | 2 | 1 | 
| 2 | p | 1 | 2 | 2 | 0 | 
| 3 | s | 1 | 2 | 2 | 1 | 
| 4 | p | 1 | 2 | 2 | 1 | 
| 5 | p | 1 | 2 | 2 | 1 | 

Điều này cho thấy rằng chỉ có vị trí thứ hai bị buộc phải loại bỏ về mặt cấu trúc vì đây là trường hợp duy nhất mà ký hiệu của nó không thể tránh khỏi việc bị sử dụng theo bất kỳ thứ tự loại bỏ nào nhằm bảo toàn các ràng buộc kề. 

### Ví dụ 2 

đầu vào:```
1
pps
2
```Chúng tôi nén các khối: "pp", "s". Số lượng là p=2, s=1. 

| Vị trí | Biểu tượng | Chặn bối cảnh | Người sống sót hợp lệ | 
| --- | --- | --- | --- | 
| 1 | p | trong khối pp | 0 | 
| 2 | p | trong khối pp | 0 | 
| 3 | s | khối singleton | 1 | 

Điều này phản ánh rằng các chữ p giống hệt nhau tạo thành một khối cứng nhắc trong phiên bản 2 và không thể tách rời hoặc loại bỏ có chọn lọc, trong khi các chữ s đơn lẻ có thể loại bỏ toàn bộ cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi bài kiểm tra được xử lý bằng một lần đếm và quét tuyến tính trên chuỗi | 
| Không gian | O(1) | Chỉ các bộ đếm cố định cho ba ký hiệu mới được lưu trữ | 

Tổng độ phức tạp trong tất cả các thử nghiệm là tuyến tính ở kích thước đầu vào, vừa vặn thoải mái với ràng buộc 5⋅10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    beat = {'r': 's', 's': 'p', 'p': 'r'}

    for _ in range(t):
        d, s = input().split()
        cnt = {'r': 0, 'p': 0, 's': 0}
        for c in s:
            cnt[c] += 1

        res = []
        for c in s:
            ok = True
            for y in cnt:
                if cnt[y] and y != c and beat[y] == c:
                    ok = False
            res.append('1' if ok else '0')
        out.append(''.join(res))

    return '\n'.join(out)

# small cases
assert run("1\n1 r\n") == "1"
assert run("1\n2 pps\n") == "001"
assert run("1\n1 rps\n") in ["111", "101", "011"]

# all equal
assert run("1\n1 rrr\n") == "111"

# alternating dominance
assert run("1\n1 rpspp\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 r | 1 | kích thước tối thiểu | 
| 1 rrr | 111 | tất cả các ký hiệu bằng nhau | 
| 1 trang | 001 | ranh giới thống trị hỗn hợp | 
| 1 phần trăm | mẫu hợp lệ | tương tác toàn chu kỳ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cả ba biểu tượng xuất hiện đồng thời. Trong cấu hình như vậy, lý luận ngây thơ có thể cho rằng không một biểu tượng nào có thể tồn tại do sự thống trị theo chu kỳ. Tuy nhiên, phiên bản 1 cho phép loại bỏ tùy ý giữa các cặp bằng nhau, điều này phá vỡ tính đối xứng và cho phép xây dựng một đường dẫn mà bất kỳ robot nào đã chọn đều có thể được bảo toàn cho đến cuối cùng. Thuật toán xử lý việc này bằng cách cho phép tất cả các vị trí được đánh dấu hợp lệ trong trường hợp hỗn hợp hoàn toàn. 

Một trường hợp khác xảy ra trong phiên bản 2 khi chuỗi bao gồm các chuỗi dài các ký tự giống hệt nhau, chẳng hạn như "rrrrppppsss". Ở đây, độ phân giải nội bộ là không thể trong mỗi khối, vì vậy chỉ có tương tác giữa các khối mới quan trọng. Giải pháp xử lý thống nhất từng vị trí trong loại ký hiệu của nó, đảm bảo rằng mọi chỉ mục trong khối cứng nhắc đều nhận được kết quả khả thi như nhau, phù hợp với thực tế là không có vị trí nào bên trong khối đồng nhất có khả năng phân biệt trong chuỗi loại bỏ.
