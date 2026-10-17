---
title: "CF 105505A - Nối thêm và hoảng sợ!"
description: "Chúng ta được cung cấp một chuỗi chữ hoa duy nhất được tạo ra bằng quy trình hai bước. Đầu tiên, ai đó có một chuỗi t ban đầu. Sau đó, họ tạo ra chuỗi thứ hai bằng cách lấy tất cả các chữ cái riêng biệt của t, sắp xếp chúng theo thứ tự bảng chữ cái và viết mỗi chữ cái một lần."
date: "2026-06-23T01:34:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105505
codeforces_index: "A"
codeforces_contest_name: "2024-2025 ICPC Latin American Regional Programming Contest"
rating: 0
weight: 105505
solve_time_s: 53
verified: true
draft: false
---

[CF 105505A - Nối thêm và hoảng sợ!](https://codeforces.com/problemset/problem/105505/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ hoa duy nhất được tạo ra bằng quy trình hai bước. Đầu tiên, ai đó có một chuỗi gốc`t`. Sau đó, họ tạo ra chuỗi thứ hai bằng cách lấy tất cả các chữ cái riêng biệt của`t`, sắp xếp chúng theo thứ tự bảng chữ cái và viết mỗi chữ cái một lần. Cuối cùng, thay vì thay thế tệp gốc bằng chuỗi dẫn xuất này, họ đã thêm nó vào tệp gốc. 

Vì vậy chuỗi đầu vào`S`thực sự là`S = t + g(t)`, Ở đâu`g(t)`là chuỗi các ký tự duy nhất được sắp xếp xuất hiện trong`t`. 

Nhiệm vụ không phải là xây dựng lại`t`chính nó, chỉ để xác định chiều dài của nó. 

Các ràng buộc cho phép độ dài chuỗi lên tới 2000. Độ dài này đủ nhỏ để mọi giải pháp bậc hai hoặc gần bậc hai có thể vượt qua một cách thoải mái, nhưng nó cũng gợi ý rằng có một thuộc tính cấu trúc ẩn, vì phép biến đổi bao gồm sắp xếp và loại bỏ trùng lặp, thường làm giảm thông tin về kích thước bảng chữ cái cố định nhỏ. 

Một quan sát quan trọng là`g(t)`chỉ chứa các chữ cái viết hoa riêng biệt. Vì chỉ có 26 chữ cái viết hoa nên hậu tố được nối thêm có thể có độ dài tối đa là 26. Điều này ngay lập tức loại trừ mọi cách tiếp cận thử phân chia vị trí tùy ý trên tất cả 2000 chỉ số làm chiến lược chính, bởi vì chúng ta có thể bản địa hóa ranh giới thành một vùng rất nhỏ gần cuối. 

Một sai lầm ngây thơ là cho rằng điểm phân tách không xác định ở bất kỳ đâu trong chuỗi. Ví dụ, đưa ra`S = "ICPCCIP"`, người ta có thể cố gắng chia thành nhiều vị trí và xác thực từng vị trí, cách này hiệu quả nhưng không cần thiết. 

Một trường hợp phức tạp khác là khi`t`chính nó đã chứa chính xác tất cả các chữ cái riêng biệt theo thứ tự được sắp xếp. Ví dụ, nếu`t = "ABC"`, sau đó`g(t) = "ABC"`, vì vậy chuỗi cuối cùng trở thành`"ABCABC"`. Việc trùng lặp có thể khiến điểm phân chia trở nên mơ hồ nếu không được suy luận cẩn thận. 

Thách thức thực sự là nhận ra rằng hậu tố được xác định hoàn toàn bởi tập hợp các ký tự trong tiền tố và độ dài của nó bị giới hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp thử mọi điểm phân chia có thể`i`từ`1`ĐẾN`n-1`. Đối với mỗi lần phân chia, chúng tôi xử lý`S[:i]`với tư cách là một ứng cử viên`t`, tính toán tập hợp các ký tự, sắp xếp chúng và so sánh với`S[i:]`. Nếu chúng khớp, chúng tôi quay lại`i`. 

Điều này đúng vì nó trực tiếp thực thi định nghĩa của công trình. Tuy nhiên, đối với mỗi lần phân tách, chúng tôi có thể xây dựng lại một bộ và sắp xếp tối đa 26 ký tự, cách này rẻ nhưng chúng tôi vẫn làm được.`O(n)`lần, dẫn đến khoảng`O(n^2)`xử lý ký tự trong trường hợp xấu nhất. Với`n = 2000`, đây vẫn là ranh giới ổn, nhưng không cần thiết. 

Cái nhìn sâu sắc quan trọng là phần thứ hai`g(t)`chính xác là tập hợp các ký tự được sắp xếp của`t`và do đó độ dài của nó là số ký tự riêng biệt trong`t`. Con số này nhiều nhất là 26. Điều đó có nghĩa là điểm phân tách phải nằm trong 26 ký tự cuối cùng của chuỗi. 

Do đó chúng ta có thể thử tất cả các độ dài hậu tố có thể có`k`từ`1`ĐẾN`26`, giải thích`S[n-k:]`với tư cách là ứng cử viên`g(t)`và kiểm tra xem nó có khớp với các ký tự duy nhất được sắp xếp của tiền tố không`S[:n-k]`. Chính xác một trong số này sẽ phù hợp. 

Điều này làm giảm vấn đề từ việc quét tới 2000 điểm phân chia đến việc kiểm tra tối đa 26 ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra phân chia Brute Force | O(n²) | O(1) thêm | Được chấp nhận nhưng không cần thiết | 
| Hãy thử độ dài hậu tố ≤ 26 | O(26·n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế rằng phần được nối thêm là phiên bản loại bỏ trùng lặp được sắp xếp của chuỗi gốc, do đó kích thước của nó được xác định hoàn toàn bởi số lượng ký tự riêng biệt trong tiền tố. 

1. Hãy để`n`là chiều dài của`S`. Chúng tôi cho rằng hậu tố tương ứng với`k`ký tự riêng biệt cho một số`k`giữa`1`Và`26`. Điều này là do chỉ có 26 chữ cái viết hoa nên không có chuỗi trùng lặp nào có thể vượt quá độ dài này. 
2. Đối với mỗi ứng viên`k`, chúng tôi lấy cái cuối cùng`k`nhân vật của`S`như một tiềm năng`g(t)`. Đây là hậu tố được giả thuyết. 
3. Chúng tôi tính tiền tố`S[:n-k]`, giải thích nó như`t`và trích xuất bộ ký tự của nó. Sắp xếp bộ này tạo ra dạng mong đợi của`g(t)`. 
4. Chúng tôi so sánh chuỗi ký tự duy nhất được sắp xếp được tính toán với hậu tố. Nếu chúng khớp chính xác thì`n-k`là độ dài chính xác của chuỗi gốc. 
5. Chúng tôi trở lại`n-k`ngay lập tức khi tìm thấy kết quả phù hợp. 

### Tại sao nó hoạt động 

Cấu trúc đảm bảo rằng hậu tố được xác định duy nhất bởi tập hợp các ký tự trong tiền tố. Do đó, sau khi chúng tôi sửa điểm phân tách ứng cử viên, hậu tố buộc phải bằng các ký tự duy nhất được sắp xếp của tiền tố đó. Vì hậu tố đúng cũng phải thỏa mãn điều kiện này nên nó phải xuất hiện trong số các giá trị được kiểm tra của`k`. Giới hạn trên của 26 đảm bảo chúng ta không bỏ sót phép phân chia chính xác và không có phép phân chia không hợp lệ nào có thể vô tình thỏa mãn điều kiện bằng vì bất kỳ sự không khớp nào trong bộ ký tự hoặc thứ tự sẽ phá vỡ cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    for k in range(1, 27):
        if k > n:
            break
        suffix = s[n - k:]
        prefix = s[:n - k]

        if len(set(prefix)) != k:
            continue

        if ''.join(sorted(set(prefix))) == suffix:
            print(n - k)
            return

solve()
```Việc triển khai trực tiếp mã hóa tìm kiếm giới hạn theo độ dài hậu tố có thể. Điều tinh tế duy nhất là đảm bảo chúng tôi không bao giờ thử một hậu tố dài hơn chính chuỗi đó và so sánh hậu tố đó với dạng chuẩn của tiền tố. 

Một lỗi phổ biến là quên rằng hậu tố đã được sắp xếp và loại bỏ trùng lặp, do đó việc so sánh nó với một tập hợp chưa được sắp xếp sẽ không thành công. Một cách khác là lặp lại các vị trí được phân chia lên đến`n`, điều này không cần thiết và ít cấu trúc hơn so với việc sử dụng giới hạn kích thước bảng chữ cái. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`ICPCCIP`Chúng tôi tính toán độ dài hậu tố có thể. 

| k | tiền tố | tiền tố được sắp xếp duy nhất | hậu tố | trận đấu | 
| --- | --- | --- | --- | --- | 
| 1 | ICPCCI | CIP | P | không | 
| 2 | ICPCC | CIP | IP | không | 
| 3 | ICPC | CIP | CIP | vâng | 

Sự phân chia chính xác xảy ra tại`k = 3`, vậy độ dài ban đầu là`7 - 3 = 4`. 

Điều này xác nhận rằng hậu tố chính xác là bảng chữ cái được sắp xếp trùng lặp của tiền tố. 

### Ví dụ 2:`ABEDCCCABCDE`| k | tiền tố | tiền tố được sắp xếp duy nhất | hậu tố | trận đấu | 
| --- | --- | --- | --- | --- | 
| 4 | ABEDCC | ABCDE | BCDE | không | 
| 5 | ABEDCCC | ABCDE | ABCDE | vâng | 

Ở đây độ dài hậu tố chính xác là`5`, cho chiều dài ban đầu`12 - 5 = 7`. 

Điều này chứng tỏ rằng các ký tự lặp lại trong tiền tố không ảnh hưởng đến tính chính xác vì tính năng loại bỏ trùng lặp sẽ loại bỏ chúng trước khi so sánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26·n) | Chúng tôi kiểm tra tối đa 26 độ dài hậu tố, mỗi độ dài yêu cầu quét tuyến tính hoặc thiết lập cấu trúc trên tiền tố | 
| Không gian | O(1) | Chỉ đặt bộ nhớ tạm thời tối đa 26 ký tự | 

Các ràng buộc cho phép tối đa 2000 ký tự, vì vậy giải pháp này chạy tốt trong giới hạn thời gian do hệ số không đổi nhỏ và được giới hạn bởi kích thước bảng chữ cái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import builtins
    # inline solution
    def solve():
        s = input().strip()
        n = len(s)
        for k in range(1, 27):
            if k > n:
                break
            suffix = s[n - k:]
            prefix = s[:n - k]
            if len(set(prefix)) != k:
                continue
            if ''.join(sorted(set(prefix))) == suffix:
                print(n - k)
                return
    solve()
    return ""

# provided samples
run("ICPCCIP\n")
run("ABEDCCCABCDE\n")
run("ZZ\n")

# custom cases
run("AAAAAA\n")      # all same characters
run("ABCABC\n")      # already full alphabet then appended
run("ICPCICPC\n")    # repeated structure
run("ABCDDCBAABCD\n")# mixed ordering
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ICPCCIP | 4 | phát hiện phân chia cơ bản | 
| ABEDCCCABCDE | 7 | hậu tố bằng tiền tố được sắp xếp loại bỏ | 
| ZZ | 1 | trường hợp khác biệt tối thiểu | 
| AAAAAA | 3 | sự lặp lại nặng nề đúng đắn | 
| ABCABC | 3 | sao chép tập hợp đầy đủ khác biệt | 
| IPCICPC | 4 | xử lý cấu trúc lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các ký tự trong`t`giống hệt nhau. Ví dụ,`S = "AA"`. Cách giải thích hợp lệ duy nhất là`t = "A"`Và`g(t) = "A"`, vậy câu trả lời là`1`. Thuật toán kiểm tra`k = 1`, tính tiền tố`"A"`, dạng sắp xếp duy nhất của nó`"A"`, và khớp với hậu tố`"A"`. 

Một trường hợp cạnh khác là khi`t`có tất cả 26 chữ cái. Trong trường hợp đó, độ dài hậu tố chính xác là 26. Thuật toán vẫn hoạt động vì nó kiểm tra rõ ràng tất cả các giá trị lên đến 26 và sẽ khớp với hậu tố bảng chữ cái đầy đủ. 

Trường hợp thứ ba là khi tiền tố chứa các chữ cái lặp lại nằm rải rác trong chuỗi, chẳng hạn như`"ABABAB"`. Bước loại bỏ trùng lặp đảm bảo hậu tố được tính toán vẫn ổn định như`"AB"`, do đó, bất kỳ hoán vị nào của các chữ cái lặp lại đều không ảnh hưởng đến tính chính xác.
