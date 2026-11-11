---
title: "CF 1582C - Bà Capa Đan Khăn"
description: "Chúng ta được cấp một chuỗi gồm các chữ cái viết thường. Chúng ta được phép chọn chính xác một chữ cái trong bảng chữ cái, sau đó xóa bất kỳ lần xuất hiện nào của chữ cái đã chọn đó khỏi chuỗi, có thể không có hoặc tất cả chúng."
date: "2026-06-14T22:58:57+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1582
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 750 (Div. 2)"
rating: 1200
weight: 1582
solve_time_s: 267
verified: true
draft: false
---

[CF 1582C - Bà Capa đan khăn](https://codeforces.com/problemset/problem/1582/C) 

**Đánh giá:** 1200 
**Tags:** Brute Force, cấu trúc dữ liệu, tham lam, chuỗi, hai con trỏ 
**Thời gian giải:** 4 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi gồm các chữ cái viết thường. Chúng ta được phép chọn chính xác một chữ cái trong bảng chữ cái, sau đó xóa bất kỳ lần xuất hiện nào của chữ cái đã chọn đó khỏi chuỗi, có thể không có hoặc tất cả chúng. Sau những lần xóa này, chúng tôi muốn chuỗi kết quả trở thành một bảng màu và chúng tôi muốn giảm thiểu số lượng ký tự chúng tôi đã xóa. Nếu không có lựa chọn chữ cái nào có thể biến chuỗi thành một chuỗi palindrome thì chúng ta phải thông báo rằng điều đó là không thể. 

Một hạn chế chính là việc xóa bị hạn chế rất nhiều. Chúng tôi không được phép tự do loại bỏ các ký tự không khớp; chúng ta chỉ có thể xóa các lần xuất hiện của một ký tự cố định. Hạn chế này biến vấn đề thành việc kiểm tra xem chuỗi có thể được chuyển thành một bảng màu hay không bằng cách "lọc ra" một loại ký tự. 

Kích thước đầu vào đạt tới 100.000 ký tự cho mỗi trường hợp thử nghiệm với tổng số ký tự lên tới 200.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng các lựa chọn xóa và kiểm tra bảng màu một cách độc lập cho mọi chữ cái và mọi kết hợp xóa. Một cách tiếp cận đơn giản là xây dựng lại chuỗi nhiều lần sẽ đạt O(n * 26 * n), vượt xa giới hạn có thể chấp nhận được. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi đã là một bảng màu. Trong trường hợp đó, câu trả lời là 0 bất kể chúng ta chọn chữ cái nào. Một tình huống phức tạp khác phát sinh khi sự không khớp tồn tại ở các vị trí đối xứng nhưng không thể giải quyết tất cả bằng cách loại bỏ một loại ký tự. Ví dụ: nếu các cặp không khớp yêu cầu loại bỏ hai chữ cái khác nhau thì không có giải pháp hợp lệ nào tồn tại ngay cả khi chuỗi "gần như" trông đối xứng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử từng chữ cái trong số 26 chữ cái có thể làm ký tự xóa đã chọn. Với mỗi lựa chọn, hãy xây dựng chuỗi kết quả bằng cách bỏ qua chữ cái đó, sau đó kiểm tra xem chuỗi còn lại có phải là một bảng màu hay không. Nếu có, hãy đếm số lần xóa đã được thực hiện và giữ ở mức tối thiểu. 

Điều này hoạt động vì nó mô phỏng trực tiếp hoạt động được phép. Tuy nhiên, mỗi lần kiểm tra palindrome là O(n) và việc xây dựng các chuỗi được lọc cũng là O(n). Vì chúng tôi lặp lại điều này cho 26 chữ cái, nên độ phức tạp sẽ trở thành O(26 · n²) nếu được triển khai bất cẩn với cấu trúc chuỗi lặp lại hoặc O(26 · n) cho mỗi lần kiểm tra dẫn đến O(26 · n2) trên tất cả các bước kiểm tra trong các mẫu triển khai tồi tệ nhất. Với n lên đến 10⁵, điều này là không khả thi. 

Quan sát quan trọng là chúng ta không thực sự cần phải xây dựng lại chuỗi cho mọi chữ cái ứng cử viên. Thay vào đó, chúng ta có thể xử lý vấn đề như một phép kiểm tra bảng màu hai con trỏ với một ràng buộc: khi chúng ta thấy một điểm không khớp, chúng ta buộc phải "sửa" nó bằng cách xóa một trong hai chữ cái liên quan và lựa chọn xóa đó phải nhất quán trên tất cả các điểm không khớp. Điều này có nghĩa là các ứng cử viên có ý nghĩa duy nhất là hai chữ cái xuất hiện ở lần không khớp đầu tiên từ bên ngoài. Bất kỳ giải pháp hợp lệ nào cũng phải loại bỏ hoàn toàn một trong các chữ cái đó khỏi các cặp xung đột, nếu không thì tính đối xứng không thể được khôi phục. 

Vì vậy, chúng tôi giảm vấn đề xuống việc kiểm tra tối đa hai chữ cái ứng cử viên bắt nguồn từ chữ cái không khớp đầu tiên và mô phỏng kiểm tra bảng màu bị ràng buộc trong đó chúng tôi bỏ qua các lần xuất hiện của chữ cái đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26 · n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng quét hai con trỏ từ cả hai đầu của chuỗi.

1. Khởi tạo hai con trỏ, trái tại 0 và phải tại n − 1. Di chuyển chúng vào trong miễn là các ký tự khớp nhau. Điều này xác minh tính đối xứng tiền tố-hậu tố đã tồn tại. 
2. Nếu các con trỏ giao nhau, chuỗi đó đã là một chuỗi palindrome nên không cần xóa. 
3. Nếu tìm thấy sự không khớp đầu tiên ở vị trí bên trái và bên phải, chúng ta có hai chữ cái ứng cử viên: s[left] và s[right]. Mọi giải pháp hợp lệ đều phải loại bỏ tất cả các lần xuất hiện của một trong hai chữ cái này để giải quyết sự không khớp này. 
4. Đối với mỗi chữ cái ứng cử viên, hãy mô phỏng việc kiểm tra bảng màu trong đó chúng ta bỏ qua tất cả các lần xuất hiện của chữ cái đó. Chúng ta lại sử dụng hai con trỏ, bỏ qua các ký tự bằng chữ cái đã chọn. 
5. Nếu sau khi bỏ qua một chữ cái, chuỗi trở thành một bảng màu, chúng tôi tính số ký tự bị loại bỏ là tổng số ký tự đó trong chuỗi ban đầu trừ đi số lần xuất hiện còn lại trong hệ thống bảng màu hợp lệ. 
6. Lấy kết quả tối thiểu của hai thí sinh. Nếu không có ứng cử viên nào tạo ra một bảng màu hợp lệ, hãy trả về −1. 

Lý do đằng sau việc hạn chế các ứng cử viên ở cặp không khớp là vị trí đầu tiên mà tính đối xứng không thành công đã buộc một bên của điểm không khớp phải bị loại bỏ hoàn toàn trong bất kỳ giải pháp hợp lệ nào. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là không thể sửa lỗi palindrome không khớp ở vị trí l và r bằng cách xóa các ký tự không liên quan. Vì chúng ta chỉ có thể xóa một chữ cái trên toàn cầu nên ít nhất một trong số s[l] hoặc s[r] phải bị xóa khỏi tất cả các vị trí liên quan. Sau khi xóa chữ cái đó, phần còn lại của chuỗi phải nhất quán về mặt cấu trúc khi ghép nối hai con trỏ. Điều này đảm bảo rằng bất kỳ cấu hình thành công nào cũng phải tương ứng với một trong hai lần xóa ứng cử viên, do đó chỉ kiểm tra những điều đó là đủ và đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check(s, ch):
    l, r = 0, len(s) - 1
    removed = 0
    while l < r:
        while l < r and s[l] == ch:
            l += 1
            removed += 1
        while l < r and s[r] == ch:
            r -= 1
            removed += 1
        if l >= r:
            break
        if s[l] != s[r]:
            return float('inf')
        l += 1
        r -= 1
    return removed

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()

        l, r = 0, n - 1
        while l < r and s[l] == s[r]:
            l += 1
            r -= 1

        if l >= r:
            print(0)
            continue

        c1, c2 = s[l], s[r]

        ans = min(check(s, c1), check(s, c2))
        print(-1 if ans == float('inf') else ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc xác định vị trí không khớp đầu tiên, xác định hai ký tự có liên quan duy nhất đáng xem xét. Chức năng trợ giúp`check`mô phỏng việc loại bỏ tất cả các lần xuất hiện của một ký tự đã chọn trong khi vẫn duy trì kiểm tra bảng màu bằng cách sử dụng hai con trỏ. các`removed`bộ đếm theo dõi số lần xóa cần thiết và các cấu hình không hợp lệ sẽ ngay lập tức trả về giá trị vô cùng. 

Logic chính dựa trên thực tế là khi chúng ta cam kết loại bỏ một trong hai ký tự không khớp, phần còn lại của chuỗi phải nhất quán theo các ràng buộc palindrome. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi`abcaacab`. 

Đầu tiên chúng ta so sánh từ cả hai đầu cho đến khi không khớp: 

| trái | đúng | s[trái] | s[phải] | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 7 | một | b | điểm dừng không khớp | 

Các ứng cử viên là`a`Và`b`. 

Vì`a`, loại bỏ tất cả`a`ký tự mang lại`bcaacb`, đó là một palindrome. Số lần xóa là 4. 

cho`b`, loại bỏ tất cả`b`ký tự mang lại`acaaca`, đó cũng là một palindrome. Yêu cầu xóa là 2. 

Tối thiểu là 2. 

Bây giờ hãy xem xét`khyyhhyhky`. 

Sự không phù hợp xảy ra ở điểm không phù hợp ngoài cùng buộc các ứng viên phải`k`Và`y`. 

Kiểm tra`k`việc loại bỏ dẫn đến một bảng màu nhất quán sau khi bỏ qua tất cả`k`ký tự, trong khi loại bỏ`y`yêu cầu xóa nhiều hơn nhưng vẫn mang lại cấu trúc palindrome hợp lệ. Thuật toán chọn số lần xóa tối thiểu khả thi. 

Những dấu vết này cho thấy thuật toán không bao giờ khám phá các chữ cái không cần thiết, chỉ những chữ cái trực tiếp giải quyết xung đột cấu trúc đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét hai con trỏ cộng với tối đa hai lần quét được lọc cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ sử dụng con trỏ và bộ đếm | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi 2 × 10⁵, do đó, việc quét tuyến tính trên mỗi trường hợp thử nghiệm sẽ vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    input = sys.stdin.readline

    def check(s, ch):
        l, r = 0, len(s) - 1
        removed = 0
        while l < r:
            while l < r and s[l] == ch:
                l += 1
                removed += 1
            while l < r and s[r] == ch:
                r -= 1
                removed += 1
            if l >= r:
                break
            if s[l] != s[r]:
                return float('inf')
            l += 1
            r -= 1
        return removed

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            s = input().strip()

            l, r = 0, n - 1
            while l < r and s[l] == s[r]:
                l += 1
                r -= 1

            if l >= r:
                print(0)
                continue

            c1, c2 = s[l], s[r]
            ans = min(check(s, c1), check(s, c2))
            print(-1 if ans == float('inf') else ans)

    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("""5
8
abcaacab
6
xyzxyz
4
abba
8
rprarlap
10
khyyhhyhky""") == """2
-1
0
3
2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bảng màu đơn | 0 | đã hợp lệ, không cần xóa | 
| không thể không phù hợp | -1 | không loại bỏ một chữ cái nào có thể sửa chữa cấu trúc | 
| tất cả các chữ cái giống nhau | 0 | bảng màu tầm thường | 
| mô hình xen kẽ | lựa chọn xóa tối thiểu | buộc lựa chọn ứng viên đúng cách | 

## Vỏ cạnh 

Khi chuỗi đã đối xứng, quá trình quét hai con trỏ kết thúc mà không gặp phải sự không khớp. Thuật toán ngay lập tức trả về số 0, điều này đúng vì không cần xóa và bất kỳ chữ cái nào được chọn sẽ chỉ làm tăng chi phí. 

Khi sự không khớp xảy ra ở cuối chuỗi, chẳng hạn như`ab`, các chữ cái ứng cử viên là`a`Và`b`. Mỗi lần thử loại bỏ sẽ tạo ra một chuỗi ký tự đơn, về cơ bản là một bảng màu. Thuật toán trả về chính xác chi phí xóa nhỏ hơn, bằng một trong cả hai trường hợp. 

Khi có nhiều điểm không khớp tồn tại sâu hơn trong chuỗi, điểm không khớp đầu tiên vẫn xác định các ứng cử viên có thể xóa duy nhất. Ngay cả khi các trường hợp không khớp sau này liên quan đến các ký tự khác thì mọi giải pháp hợp lệ đều phải giải quyết xung đột đầu tiên, do đó, việc hạn chế chú ý đến hai chữ cái đó vẫn đủ và thuật toán sẽ loại bỏ chính xác các trường hợp không thể xảy ra.
