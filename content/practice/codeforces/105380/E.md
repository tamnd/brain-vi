---
title: "CF 105380E - Trò chơi chuỗi Palindrome"
description: "Chúng tôi được cung cấp một chuỗi và nhiều truy vấn độc lập. Mỗi truy vấn chỉ định một đoạn của chuỗi và chúng ta chỉ được phép nhìn vào bên trong đoạn đó."
date: "2026-06-23T16:05:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105380
codeforces_index: "E"
codeforces_contest_name: "TSEC Round 1 (Div. 4)"
rating: 0
weight: 105380
solve_time_s: 100
verified: true
draft: false
---

[CF 105380E - Trò chơi chuỗi Palindrome](https://codeforces.com/problemset/problem/105380/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi và nhiều truy vấn độc lập. Mỗi truy vấn chỉ định một đoạn của chuỗi và chúng ta chỉ được phép nhìn vào bên trong đoạn đó. Nhiệm vụ của chúng ta là tìm một chuỗi con liền kề hoàn toàn chứa trong phân đoạn được truy vấn, là một chuỗi palindrome và có độ dài tối đa có thể. Câu trả lời cho mỗi truy vấn chỉ là độ dài của bảng màu tốt nhất đó. 

Vì vậy, về mặt khái niệm, mỗi truy vấn xác định một cửa sổ trên chuỗi và bên trong cửa sổ đó, chúng tôi đang tìm kiếm mẫu đối xứng mạnh nhất. 

Các ràng buộc nhỏ trên mỗi trường hợp thử nghiệm, với độ dài chuỗi và số lượng truy vấn lên tới 1000, nhưng tổng của tất cả các trường hợp thử nghiệm có thể đạt tới khoảng một triệu phép tính có dạng n lần q. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại thông tin palindrome từ đầu cho mỗi truy vấn hoặc cố gắng mở rộng xung quanh mọi trung tâm cho mọi truy vấn. Một giải pháp gần đúng bậc hai cho mỗi truy vấn sẽ quá chậm. 

Khó khăn chính là các palindromes là cấu trúc toàn cục, nhưng các truy vấn lại là những hạn chế cục bộ. Một palindrome tồn tại trong chuỗi chỉ hữu ích cho một truy vấn nếu nó được chứa đầy đủ trong khoảng của nó. 

Một số trường hợp đặc biệt cần được làm rõ. 

Nếu khoảng truy vấn có độ dài 1 thì câu trả lời luôn là 1 vì mỗi ký tự đơn lẻ là một bảng màu. Một giải pháp ngây thơ chỉ xem xét các palindrome có độ dài chẵn có thể trả về 0 không chính xác ở đây. 

Nếu chuỗi có tất cả các ký tự giống nhau thì mỗi chuỗi con là một chuỗi màu nhạt, do đó mỗi truy vấn sẽ trả về độ dài khoảng đầy đủ. Bất kỳ giải pháp nào chỉ theo dõi các bảng màu tối đa xung quanh các trung tâm nhưng không tổng hợp được giữa các vị trí sẽ đánh giá thấp trường hợp này. 

Nếu bảng màu tốt nhất lớn nhưng nằm ngoài phạm vi truy vấn một chút thì nó không được tính. Ví dụ, trong`abacaba`, bảng màu`abacaba`là toàn cầu, nhưng truy vấn`[2,6]`chỉ nên xem xét`bacab`, không phải là chuỗi đầy đủ. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp xử lý từng truy vấn một cách độc lập. Đối với một truy vấn cố định`[l, r]`, chúng tôi liệt kê tất cả các chuỗi con bên trong khoảng đó và kiểm tra xem mỗi chuỗi con có phải là một bảng màu hay không, giữ nguyên độ dài tối đa. 

Việc kiểm tra chuỗi con xem có phải là chuỗi palindrome có thể được thực hiện theo thời gian tuyến tính theo độ dài của chuỗi đó hoặc được tính toán trước bằng lập trình động. Dù bằng cách nào, đối với một truy vấn duy nhất, chúng tôi đang xem xét đại khái$O((r-l+1)^3)$nếu được thực hiện một cách ngây thơ, hoặc$O((r-l+1)^2)$với kiểm tra DP. Với tối đa 1000 truy vấn, điều này trở nên quá chậm trong trường hợp xấu nhất. 

Cấu trúc giúp giải quyết được vấn đề này là tư cách thành viên palindrome cho bất kỳ chuỗi con nào đều có thể được tính toán trước một lần trong$O(n^2)$, sau đó được sử dụng lại. Thử thách còn lại không phải là phát hiện các palindrome mà là trả lời các truy vấn có phạm vi tối đa trên một tập hợp các “khoảng palindrome” được tính toán trước. 

Chúng tôi lật quan điểm. Thay vì hỏi “đối với mỗi truy vấn, palindrome nào là tốt nhất?”, chúng tôi liệt kê tất cả các chuỗi con palindrome một lần. Mỗi palindrome trở thành một khoảng có giá trị bằng độ dài của nó. Sau đó, mỗi truy vấn sẽ yêu cầu khoảng giá trị lớn nhất chứa đầy đủ trong`[l, r]`. 

Điều này biến vấn đề thành một truy vấn tối đa phạm vi hai chiều theo các khoảng thời gian, mà chúng ta có thể cơ cấu lại thành tập hợp mỗi lần bắt đầu hoặc mỗi lần kết thúc và trả lời hiệu quả với các bảng tiền xử lý và bảng thưa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn |$O(n^3)$mỗi truy vấn trường hợp xấu nhất |$O(1)$hoặc$O(n^2)$| Quá chậm | 
| Tính toán trước palindromes + cấu trúc tối đa phạm vi |$O(n^2 \log n)$tiền xử lý,$O(1)$mỗi truy vấn |$O(n^2 \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp theo từng lớp, bắt đầu từ việc phát hiện bảng màu và kết thúc bằng việc trả lời truy vấn nhanh. 

### Bước 1: Tính toán trước bảng palindrome 

Chúng tôi xây dựng một bảng`pal[i][j]`điều đó cho biết liệu chuỗi con`s[i..j]`là một palindrome. Điều này được thực hiện bằng cách sử dụng DP tiêu chuẩn: các ký tự đơn là palindrome, chuỗi con hai ký tự là palindrome nếu bằng nhau và chuỗi con dài hơn phụ thuộc vào điểm cuối và chuỗi con bên trong. 

Điều này cung cấp cho chúng ta kiến ​​thức đầy đủ về chuỗi con nào là palindrome hợp lệ. 

### Bước 2: Chuyển đổi palindrome thành giá trị “kết thúc tốt nhất ở r” 

Đối với mỗi điểm cuối`r`, chúng tôi muốn biết, với mọi vị trí bắt đầu có thể`l`, palindrome dài nhất kết thúc ở`r`và bắt đầu vào hoặc sau`l`. 

Chúng tôi xác định một mảng`bestStart[r][l]`ý nghĩa: trong số tất cả các palindrome kết thúc tại`r`và bắt đầu vào`[l, r]`, độ dài tối đa là bao nhiêu. 

Để tính toán điều này, chúng tôi quét`l`từ phải sang trái cho mỗi cố định`r`. Nếu như`s[l..r]`là một palindrome, nó đóng góp độ dài ứng cử viên`r-l+1`. Trong khi di chuyển sang trái, chúng tôi duy trì tốc độ chạy tối đa để mỗi`bestStart[r][l]`có thể được điền vào thời gian không đổi sau khi kiểm tra`pal[l][r]`. 

Bước này nén tất cả các palindromes kết thúc tại`r`thành một cấu trúc có thể trả lời “palindrome tốt nhất kết thúc ở đây dưới một ràng buộc bên trái”. 

### Bước 3: Chuyển truy vấn thành phạm vi tối đa trên các điểm cuối 

Đối với một truy vấn`[l, r]`, bảng màu chúng ta chọn phải kết thúc ở đâu đó giữa`l`Và`r`. Đối với mỗi điểm cuối có thể`e`, chúng ta đã biết palindrome tốt nhất kết thúc ở`e`bắt đầu vào hoặc sau`l`, đó là`bestStart[e][l]`. 

Vì vậy, câu trả lời cho truy vấn trở thành: 

tối đa trên tất cả`e`TRONG`[l, r]`của`bestStart[e][l]`. 

Bây giờ vấn đề là truy vấn phạm vi tối đa trên một mảng cố định trên mỗi`l`. 

### Bước 4: Xử lý trước truy vấn phạm vi tối đa 

Đối với mỗi cố định`l`, chúng tôi xây dựng một bảng thưa thớt trên`e`cho mảng`bestStart[e][l]`. Điều này cho phép trả lời các truy vấn tối đa trong bất kỳ khoảng thời gian nào`[l, r]`trong thời gian không đổi. 

Từ`n`nhiều nhất là 1000, đang xây dựng`n`bảng thưa thớt là đủ hiệu quả. 

## Tại sao nó hoạt động 

Mỗi chuỗi con palindrome được xác định duy nhất bởi các điểm cuối của nó`(i, j)`. Thuật toán đảm bảo rằng mỗi chuỗi con như vậy được xem xét chính xác một lần trong`bestStart[j][i]`và không có bảng màu hợp lệ nào bị bỏ sót. 

Đối với bất kỳ truy vấn nào, mỗi palindrome ứng viên được nhóm theo vị trí kết thúc của nó và cấu trúc đảm bảo rằng chúng ta chỉ lấy cực đại trên các vị trí bắt đầu hợp lệ. Bảng thưa thớt đảm bảo chúng ta không bỏ qua bất kỳ điểm cuối nào trong`[l, r]`, do đó giá trị tối đa trên tất cả các palindrome hợp lệ luôn được bảo toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()
        s = " " + s  # 1-indexed

        pal = [[False] * (n + 1) for _ in range(n + 1)]
        best_start = [[0] * (n + 2) for _ in range(n + 2)]

        for i in range(1, n + 1):
            pal[i][i] = True

        for i in range(1, n):
            if s[i] == s[i + 1]:
                pal[i][i + 1] = True

        for length in range(3, n + 1):
            for i in range(1, n - length + 2):
                j = i + length - 1
                if s[i] == s[j] and pal[i + 1][j - 1]:
                    pal[i][j] = True

        for r in range(1, n + 1):
            for l in range(r, 0, -1):
                best_start[r][l] = best_start[r][l + 1]
                if pal[l][r]:
                    best_start[r][l] = max(best_start[r][l], r - l + 1)

        LOG = 10
        st = [[[0] * (LOG + 1) for _ in range(n + 1)] for _ in range(n + 1)]

        for l in range(1, n + 1):
            for r in range(1, n + 1):
                st[l][r][0] = best_start[r][l]

            j = 1
            while (1 << j) <= n:
                for r in range(1, n + 1):
                    if r + (1 << j) - 1 <= n:
                        st[l][r][j] = max(st[l][r][j - 1],
                                          st[l][r + (1 << (j - 1))][j - 1])
                j += 1

        def query(l, r):
            length = r - l + 1
            k = (length).bit_length() - 1
            return max(st[l][r][k], st[l][r - (1 << k) + 1][k])

        for _ in range(q):
            l, r = map(int, input().split())
            print(query(l, r))

if __name__ == "__main__":
    solve()
```Đoạn mã đầu tiên xây dựng một bảng DP palindrome tiêu chuẩn. Sau đó, nó nén tất cả các palindrome theo điểm cuối vào`best_start`. Cuối cùng, nó xây dựng một bảng thưa cho từng ranh giới bên trái cố định để mỗi truy vấn trở thành truy vấn tối đa trong phạm vi thời gian không đổi trên các điểm cuối. 

Một cạm bẫy phổ biến ở đây là cố gắng trả lời các truy vấn trực tiếp từ`pal[i][j]`mà không theo dõi những palindrome nào thực sự nằm bên trong`[l, r]`. Điều đó làm mất đi sự phụ thuộc vào việc ngăn chặn. Việc tách thành “tổng hợp dựa trên mục đích cuối cùng” là điều làm cho giải pháp trở nên nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`abcddcba`, truy vấn`[1, 8]`Chúng tôi chỉ hiển thị các bước chính. 

| r | bestStart[r][l] mục liên quan | 
| --- | --- | 
| 5 (`d`) | ký tự đơn | 
| 6 (`d`) | xuôi ngược đều giống nhau`dd`cho giá trị 2 tại l=5 | 
| 7 (`c`) | độc thân | 
| 8 (`a`) | cấu trúc đảo ngược hoàn toàn đóng góp nhưng bị giới hạn | 

Truy vấn kiểm tra tất cả các điểm cuối từ 1 đến 8 và tìm giá trị tối đa 8 từ`abcddcba`. 

Dấu vết này cho thấy các palindrome toàn dải chiếm ưu thế như thế nào khi chúng tồn tại. 

### Ví dụ 2 

Chuỗi đầu vào:`abcddcba`, truy vấn`[2, 7]`Chúng tôi giới hạn điểm cuối ở mức 2..7. Bảng màu tốt nhất đầy đủ bên trong là`cddc`với chiều dài 4. 

| điểm cuối e | bestStart[e][2] | 
| --- | --- | 
| 4 | 0 | 
| 5 | 0 | 
| 6 | 2 (`dd`) | 
| 7 | 1 | 

Tối đa là 4 từ`cddc`. 

Điều này xác nhận rằng các palindrome hợp lệ phải đáp ứng cả ràng buộc điểm cuối và ngăn chặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \log n)$mỗi bài kiểm tra | DP cho palindromes là$O(n^2)$, xây dựng các bảng thưa thớt sẽ thêm hệ số log | 
| Không gian |$O(n^2 \log n)$| lưu trữ bảng palindrome, bestStart và bảng thưa thớt | 

Với$n \le 1000$và tổng cộng$n \cdot q \le 10^6$, điều này phù hợp thoải mái trong giới hạn vì các truy vấn được trả lời trong thời gian không đổi sau khi xử lý trước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert run("""2
8 5
abcddcba
1 8
2 7
2 8
4 4
3 6
5 3
aaabb
1 5
4 5
2 3
""") == """8
6
6
1
4
3
2
2"""

# all identical
assert run("""1
5 2
aaaaa
1 5
2 4
""") == """5
3"""

# single char queries
assert run("""1
3 3
abc
1 1
2 2
3 3
""") == """1
1
1"""

# smallest mixed
assert run("""1
4 2
abca
1 4
2 3
""") == """3
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chuỗi giống hệt nhau | câu trả lời đầy đủ | sự thống trị palindrome toàn cầu | 
| ký tự đơn | luôn 1 | trường hợp cơ sở đúng đắn | 
| trộn chuỗi nhỏ | chỉ palindromes địa phương | logic ngăn chặn | 

## Vỏ cạnh 

Đối với một chuỗi ký tự giống hệt nhau như`aaaaa`, mỗi chuỗi con là một palindrome. Thuật toán đánh dấu mọi`(l, r)`hợp lệ trong bảng DP, và`best_start`truyền bá toàn bộ chiều dài. Khi một truy vấn hỏi`[2, 4]`, tập hợp điểm cuối đảm bảo tối đa`3`được trả về từ chuỗi con`aaa`, xác nhận việc xử lý chính xác các vùng palindrome dày đặc. 

Đối với các truy vấn một ký tự như`[3, 3]`TRONG`abc`, chỉ các mục đường chéo`pal[i][i]`là đúng. các`best_start`mảng ghi lại độ dài 1 tại mỗi vị trí và truy vấn trên một điểm cuối duy nhất trả về chính xác 1, khớp với định nghĩa rằng mỗi ký tự là một bảng màu. 

Đối với các truy vấn trong đó bảng màu tối ưu trải dài qua trung tâm nhưng không vượt qua các ranh giới, chẳng hạn như`abacaba`với`[2, 6]`, toàn bộ bảng màu sẽ tự động bị loại trừ vì các điểm cuối của nó nằm ngoài khoảng. Chỉ các palindrome chứa đầy đủ trong phân đoạn mới đóng góp vào`best_start`, do đó kết quả sẽ giảm chính xác thành chuỗi con đối xứng bên trong lớn nhất.
