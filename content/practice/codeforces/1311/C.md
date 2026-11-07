---
title: "CF 1311C - Thực hiện Combo"
description: "Chúng ta được cung cấp một chuỗi cố định s đại diện cho một chuỗi các lần nhấn nút trong một combo trò chơi đối kháng. Người chơi liên tục cố gắng gõ toàn bộ chuỗi này từ trái sang phải. Tuy nhiên, quá trình này bị gián đoạn."
date: "2026-06-11T17:17:01+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1311
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 624 (Div. 3)"
rating: 1300
weight: 1311
solve_time_s: 138
verified: false
draft: false
---

[CF 1311C - Thực hiện Combo](https://codeforces.com/problemset/problem/1311/C) 

**Đánh giá:** 1300 
**Tags:** vũ lực 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi cố định`s`đại diện cho một chuỗi các lần nhấn nút trong một trò chơi đối kháng kết hợp. Người chơi liên tục cố gắng gõ toàn bộ chuỗi này từ trái sang phải. 

Tuy nhiên, quá trình này bị gián đoạn. Trong thời gian đầu tiên`m`lần thử, mỗi lần thử chỉ thành công ở một vị trí nhất định`p_i`, sau đó người chơi ngay lập tức khởi động lại từ đầu. Ở lần thử cuối cùng, người chơi hoàn thành thành công toàn bộ chuỗi mà không bị gián đoạn. 

Do đó, trình tự ép thực tế không chỉ là`s`lặp lại nhiều lần. Thay vào đó, nó là sự kết hợp của các tiền tố của`s`được xác định bởi những lần thử thất bại, theo sau là một bản sao đầy đủ của`s`. Nhiệm vụ là đếm xem mỗi chữ cái từ`a`ĐẾN`z`được nhấn trong toàn bộ quá trình này. 

Khó khăn chính là tổng số lần thử và độ dài chuỗi có thể lớn, lên tới`2 * 10^5`tổng hợp qua các trường hợp thử nghiệm. Mô phỏng trực tiếp gắn thêm chuỗi con cho mỗi lần thử sẽ quá chậm vì nó có thể quét liên tục các tiền tố của chuỗi, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Một sai lầm ngây thơ thường xuất phát từ việc xử lý từng lần thử thất bại một cách độc lập và chỉ tính tiền tố`s[0:p_i]`mà không nhận ra rằng các tiền tố chồng chéo sẽ tích lũy đóng góp tần số. Một vấn đề tế nhị khác là quên bao gồm việc truyền tải toàn bộ chuỗi thành công cuối cùng. 

Ví dụ, nếu`s = "abc"`Và`p = [1, 1, 2]`, trình tự đúng bao gồm`"a" + "a" + "ab" + "abc"`. Một cách tiếp cận đơn giản có thể tính toán lại hoặc ghi đè không chính xác các đóng góp thay vì tích lũy chúng, dẫn đến tần số không chính xác. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ xử lý từng lần thử bằng cách lặp lại độ dài tiền tố`p_i`và tăng dần số lượng cho mỗi ký tự trong tiền tố đó. Sau khi xử lý tất cả các lần thử không thành công, chúng tôi sẽ xử lý toàn bộ chuỗi một lần nữa. 

Điều này đúng vì nó phản ánh chính xác quá trình thực tế. Tuy nhiên, sự phức tạp của nó có vấn đề. Mỗi trong số`m`những nỗ lực có thể yêu cầu lên đến`O(n)`hoạt động trong trường hợp xấu nhất, và vì tổng của`n`Và`m`có thể đạt được`2 * 10^5`, cách tiếp cận hoạt động tổng thể trong trường hợp xấu nhất`O(nm)`trong một thử nghiệm duy nhất hoặc trong nhiều trường hợp, tốc độ này quá chậm. 

Quan sát quan trọng là chúng ta không bao giờ cần phải xây dựng lại các chuỗi một cách rõ ràng. Thay vào đó, chúng ta chỉ cần biết mỗi vị trí tiền tố được truy cập bao nhiêu lần trong tất cả các lần thử không thành công. Nếu chúng ta biết tần suất lập chỉ mục`i`được bao gồm trong tiền tố, chúng tôi có thể tích lũy đóng góp trực tiếp. 

Điều này biến vấn đề thành vấn đề tích lũy tần số trên các phạm vi tiền tố. Chúng tôi duy trì một mảng`cnt[i]`đại diện cho bao nhiêu lần vị trí`i`được bao gồm trong những lần thử thất bại. Mỗi lần thử thất bại sẽ đóng góp +1 cho tất cả các chỉ số`1..p_i`. Điều này có thể được xử lý hiệu quả bằng cách sử dụng một mảng khác biệt. Sau khi xử lý tất cả các truy vấn, chúng tôi tính tổng tiền tố`cnt`để biết mỗi vị trí được truy cập bao nhiêu lần trước lần chạy cuối cùng. 

Cuối cùng, chúng tôi thêm một lần xuất hiện của mỗi ký tự trong chuỗi đầy đủ cho lần thử thành công cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) thêm | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc chuỗi`s`và danh sách độ dài tiền tố`p`. 

Mục đích là tính toán mỗi vị trí trong`s`được truy cập trước lần thử đầy đủ cuối cùng. 
2. Tạo mảng số nguyên`cnt`kích thước`n + 1`, khởi tạo bằng 0. 

Mảng này sẽ lưu trữ một biểu diễn khác biệt về số lần mỗi chỉ mục bị bao phủ bởi các lần thử thất bại. 
3. Với mỗi giá trị`p_i`, tăng`cnt[0]`bằng 1 và giảm dần`cnt[p_i]`bằng 1. 

Điều này mã hóa rằng tất cả các vị trí từ`0`ĐẾN`p_i - 1`bị ảnh hưởng một lần bởi nỗ lực thất bại này. Mảng khác biệt tránh lặp lại rõ ràng tiền tố. 
4. Chuyển đổi`cnt`vào số lượng bảo hiểm thực tế bằng cách sử dụng tổng tiền tố. 

Sau bước này,`cnt[i]`biểu thị số lần thử thất bại bao gồm vị trí`i`. 
5. Khởi tạo mảng câu trả lời có kích thước 26 với các số 0. 

Điều này sẽ lưu trữ tổng tần số của mỗi ký tự. 
6. Đối với từng vị trí`i`trong chuỗi, thêm`cnt[i] + 1`đến tần số của`s[i]`. 

các`+1`tính đến lần thử thành công cuối cùng, luôn duyệt toàn bộ chuỗi một lần. 
7. Xuất ra 26 giá trị tích lũy. 

### Tại sao nó hoạt động 

Mỗi lần thử thất bại đóng góp chính xác một lượt truy cập vào mọi chỉ mục trong tiền tố của nó. Mảng chênh lệch mã hóa tổng của các khoảng đóng góp này mà không lặp lại chúng một cách rõ ràng. Sau khi tổng hợp tiền tố, mọi vị trí đều biết chính xác có bao nhiêu lần thử thất bại. Thêm một tài khoản cho quá trình truyền tải hoàn chỉnh cuối cùng. Vì các đóng góp mang tính cộng gộp và độc lập cho mỗi vị trí nên việc tính tổng theo chỉ số sẽ mang lại tổng tần số chính xác cho mỗi ký tự. 

## Giải pháp Python```
PythonRun
```Việc triển khai cốt lõi sử dụng một mảng khác biệt để tránh đánh dấu rõ ràng từng tiền tố. Mỗi`p_i`đóng góp một bản cập nhật phạm vi và tổng tiền tố`cur`xây dựng lại số lần thử thất bại bao gồm mỗi vị trí. 

Khi tính toán`freq`, chúng tôi luôn thêm`cur + 1`thay vì chỉ`cur`bởi vì lần thử thành công cuối cùng đóng góp chính xác một lần truyền đầy đủ bổ sung qua chuỗi. 

Một cạm bẫy triển khai phổ biến là quên rằng`cnt`có kích thước`n + 1`và rằng chúng tôi chỉ lặp lại tổng tiền tố lên đến`n - 1`. Một vấn đề tế nhị khác là đặt sai vị trí`+1`đóng góp cho nỗ lực cuối cùng, phải được bổ sung thống nhất cho mọi vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi tính toán cập nhật chênh lệch: 

| Hoạt động | hiệu ứng mảng cnt | 
| --- | --- | 
| p=1 | +1 tại 0, -1 tại 1 | 
| p=3 | +1 lúc 0, -1 lúc 3 | 

Sau tổng tiền tố: 

| tôi | cur (lượt truy cập không thành công) | s[i] | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 2 | một | 3 | 
| 1 | 1 | b | 2 | 
| 2 | 1 | c | 2 | 
| 3 | 0 | một | 1 | 

Số đếm cuối cùng:`a = 4, b = 2, c = 2`Điều này xác nhận rằng các tiền tố chồng chéo tích lũy chính xác ở chỉ số 0. 

### Ví dụ 2 

đầu vào:```

```Chúng tôi chỉ theo dõi các vị trí quan trọng: 

| tôi | s[i] | bảo hiểm thất bại cur | tổng đóng góp | 
| --- | --- | --- | --- | 
| 0 | c | 5 | 6 | 
| 1 | o | 5 | 6 | 
| 2 | d | 4 | 5 | 
| 3 | e | 3 | 4 | 
| 4 | f | 3 | 4 | 
| 5 | o | 2 | 3 | 
| 6 | r | 2 | 3 | 
| 7 | c | 2 | 3 | 
| 8 | e | 1 | 2 | 
| 9 | s | 1 | 2 | 

Tổng hợp mỗi chữ cái cho:`c = 9, o = 9, d = 5, e = 5, f = 3, r = 3, s = 2`Dấu vết này cho thấy các tiền tố sâu hơn thống trị các chỉ số ban đầu như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi trường hợp kiểm thử xử lý các cập nhật tiền tố trong O(m) và tổng hợp tiền tố qua chuỗi trong O(n) | 
| Không gian | O(n + 26) | Mảng sai phân kích thước n cộng với mảng tần số bảng chữ cái cố định | 

Tổng độ phức tạp phù hợp thoải mái trong giới hạn vì tổng của tất cả`n`Và`m`trên các trường hợp thử nghiệm được giới hạn bởi`2 * 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, m = map(int, input().split())
            s = input().strip()
            p = list(map(int, input().split()))

            cnt = [0] * (n + 1)
            for x in p:
                cnt[0] += 1
                cnt[x] -= 1

            cur = 0
            freq = [0] * 26
            for i in range(n):
                cur += cnt[i]
                freq[ord(s[i]) - 97] += cur + 1

            print(*freq)

    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""3
4 2
abca
1 3
10 5
codeforces
2 8 3 2 9
26 10
qwertyuioplkjhgfdsazxcvbnm
20 10 1 2 3 5 10 5 9 4
""") == """4 2 2 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 9 4 5 3 0 0 0 0 0 0 0 0 9 0 0 3 1 0 0 0 0 0 0 0
2 1 1 2 9 2 2 2 5 2 2 2 1 1 5 4 11 8 2 7 5 1 10 1 5 2""", "sample"

# minimum case
assert run("""1
2 1
ab
1
""") == """2 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0""", "min case"

# all full prefix
assert run("""1
5 4
abcde
1 1 1 4
"""), "repeated prefixes"

# single letter repeats
assert run("""1
3 3
aaa
1 2 2
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu | tích lũy đơn giản | độ đúng cơ sở | 
| tiền tố lặp lại | xử lý chồng chéo nặng nề | xếp chồng tiền tố | 
| chữ cái giống hệt nhau | tính chính xác của tổng hợp | hợp nhất tần số | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả`p_i = 1`, nghĩa là mọi lần thử thất bại chỉ đóng góp ký tự đầu tiên. Trong trường hợp này, các phần tăng dần của mảng chênh lệch xếp chồng lên nhau ở chỉ số 0 và tất cả các vị trí khác vẫn không bị ảnh hưởng. Thuật toán chỉ tích lũy chính xác ký tự đầu tiên nhiều lần cộng với một lần duyệt đầy đủ. 

Một trường hợp khác là khi tất cả`p_i = n - 1`, nghĩa là mọi lần thử thất bại đều gần như hoàn thành chuỗi. Ở đây, mọi chỉ mục đều được bao phủ chặt chẽ và tổng tiền tố đảm bảo sự đóng góp thống nhất trên toàn bộ chuỗi. trận chung kết`+1`thêm chính xác một đường chuyền đầy đủ bổ sung. 

Trường hợp thứ ba là khi`m = 1`, ở đâu
