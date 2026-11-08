---
title: "CF 1385D - a-Dây Tốt"
description: "Nhiệm vụ là chuyển đổi một chuỗi đã cho thành một cấu trúc được xác định đệ quy rất cụ thể tập trung quanh chữ 'a'."
date: "2026-06-16T14:21:24+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "divide-and-conquer", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1385
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 656 (Div. 3)"
rating: 1500
weight: 1385
solve_time_s: 524
verified: false
draft: false
---

[CF 1385D - a-Good String](https://codeforces.com/problemset/problem/1385/D) 

**Đánh giá:** 1500 
**Tags:** bitmasks, vũ lực, chia để trị, dp, thực hiện 
**Thời gian giải:** 8 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là chuyển đổi một chuỗi đã cho thành một cấu trúc được xác định đệ quy rất cụ thể tập trung quanh chữ cái.`'a'`. Cấu trúc không phải là tùy ý: mỗi chuỗi được đánh giá là “tốt” đối với một ký tự nhất định`c`, trong đó chuỗi là một ký tự đơn bằng`c`, hoặc nó chia thành hai nửa chiều dài bằng nhau`n/2`và chính xác một nửa bị buộc phải là tất cả`c`, trong khi nửa còn lại phải đáp ứng định nghĩa tương tự nhưng đối với ký tự tiếp theo trong bảng chữ cái. 

Vì vậy, mục tiêu không chỉ là “làm cho chuỗi đồng nhất” hoặc “sắp xếp chuỗi”, mà còn là xây dựng một cây phân rã nhị phân trên chuỗi trong đó mỗi cấp độ gán một ràng buộc ký tự cho chính xác một bên và ràng buộc đó tăng lên khi chúng ta đi sâu hơn. 

Mỗi thao tác cho phép thay đổi một ký tự tùy ý và mục tiêu là giảm thiểu số lượng vị trí chúng ta sửa đổi để toàn bộ cấu trúc trở nên hợp lệ bắt đầu từ`'a'`. 

Thực tế cấu trúc quan trọng là độ dài chuỗi luôn là lũy thừa của hai, do đó, mỗi khoảng chia đôi rõ ràng nhiều lần cho đến kích thước một. 

Từ góc độ ràng buộc, tổng độ dài của tất cả các trường hợp thử nghiệm tối đa là 2e5. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại một số bậc hai cho mỗi trường hợp thử nghiệm hoặc khám phá tất cả các chuỗi con một cách rõ ràng. Việc tính toán lại đệ quy đơn giản trên mỗi nút mà không có bộ nhớ đệm sẽ truy cập lại các phân đoạn giống nhau nhiều lần và vượt quá giới hạn. 

Trường hợp cạnh tinh vi xuất hiện khi một phân đoạn đã đáp ứng một phần nhiều cách diễn giải hợp lệ. Ví dụ: một phân khúc có thể đã đồng nhất ở một nửa nhưng không phải ở nửa còn lại và một người sửa lỗi cục bộ tham lam có thể chọn sai phía để "sửa trước", dẫn đến tổng số không tối ưu. Độ sâu và cấu trúc đệ quy giúp dễ dàng đếm hai lần các chỉnh sửa nếu các bài toán con chồng chéo không được phân tách cẩn thận. 

Một cạm bẫy khác là giả định rằng chúng ta có thể độc lập quyết định từng phần một cách tham lam. Ví dụ, nếu một nửa gần trở thành`'c'`-đồng nhất nhưng việc chọn nó sẽ gây ra chi phí đệ quy cao hơn, một lựa chọn phân chia tham lam ngây thơ đã thất bại. Quyết định phải xem xét cả hai khả năng ở mọi nút. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là mô phỏng trực tiếp định nghĩa. Đối với mỗi phân đoạn và mỗi ký tự`c`, chúng tôi cố gắng tạo phân đoạn`c`-tốt bằng cách đệ quy chọn nửa bên trái hoặc bên phải để trở thành tất cả`c`và giải quyết đệ quy nửa còn lại cho`c+1`. Ở mỗi bước, chúng ta đếm xem cần thay đổi bao nhiêu ký tự để buộc một nửa thành một chuỗi không đổi. 

Điều này đúng nhưng cực kỳ dư thừa. Mỗi đoạn`[l, r]`cho mỗi nhân vật`c`được tính toán lại nhiều lần từ đầu, mặc dù các bài toán con giống nhau xuất hiện lặp đi lặp lại trong các nhánh đệ quy khác nhau. Trong trường hợp xấu nhất, điều này dẫn đến gần như`O(n log n * 26)`các trạng thái, nhưng với sự tính toán lại nhiều trong các quá trình chuyển đổi, nó hoạt động gần hơn với đệ quy hàm mũ mà không cần ghi nhớ. 

Cái nhìn sâu sắc quan trọng là vấn đề có cấu trúc phân chia và chinh phục rất rõ ràng. Mỗi trạng thái được xác định đầy đủ bởi hai tham số: đoạn`[l, r]`và ký tự cần thiết`c`. Điều đó chỉ mang lại`O(n log n * 26)`tổng thể có các trạng thái riêng biệt. Mỗi trạng thái có thể được tính toán theo thời gian không đổi từ hai trạng thái con của nó. Điều này biến phép đệ quy thành một DP cổ điển qua quá trình phân tách giống như cây phân đoạn. 

Ở mỗi phân đoạn, chúng tôi tính toán hai khả năng: bắt buộc nửa bên trái phải đồng nhất và lặp lại ở bên phải hoặc thực thi nửa bên phải và lặp lại ở bên trái. Chi phí để tạo ra một nửa đồng phục chỉ đơn giản là tính những điểm không khớp với ký tự mục tiêu, có thể được tính toán trước hoặc tính toán nhanh chóng vì mỗi phần tử được truy cập theo logarit. 

Điều này làm giảm vấn đề thành đệ quy từ dưới lên hoặc được ghi nhớ từ trên xuống trên cây phân rã nhị phân đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n * 2^k) trường hợp xấu nhất do tính toán lại | O(n log n) đệ quy | Quá chậm | 
| DP tối ưu | O(n log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi chuỗi là một đoạn được chia thành nhiều lần thành hai nửa và ở mỗi cấp độ, chúng tôi quyết định nửa nào sẽ trở thành đồng nhất cho ký tự hiện tại. 

1. Định nghĩa hàm đệ quy`solve(l, r, c)`trả về số lượng thay đổi tối thiểu cần thiết để thực hiện`s[l:r]`Một`c`-phân đoạn tốt. nhân vật`c`đại diện cho chữ cái được yêu cầu hiện tại. 
2. Nếu độ dài đoạn là 1, câu trả lời đơn giản là liệu`s[l] == c`. Nếu nó khớp, chi phí là 0, nếu không chi phí là 1. Đây là trường hợp cơ bản của đệ quy. 
3. Chia đoạn đó thành hai nửa bằng nhau`[l, mid]`Và`[mid+1, r]`. 
4. Tính chi phí để làm cho nửa bên trái hoàn toàn bằng`c`bằng cách đếm những điểm không khớp trong phạm vi đó. Gọi đây`cost_left_uniform`. 
5. Tính chi phí để làm ra một nửa bên phải hoàn toàn bằng`c`, tương tự sản xuất`cost_right_uniform`. 
6. Tính toán đệ quy: 

- Cách 1: nửa bên trái đồng nhất`c`, nửa bên phải là`(c+1)`-tốt, vậy tổng chi phí là`cost_left_uniform + solve(mid+1, r, c+1)`- Cách 2: nửa bên phải đồng đều`c`, nửa bên trái là`(c+1)`-tốt, vậy tổng chi phí là`cost_right_uniform + solve(l, mid, c+1)`7. Trả về giá trị tối thiểu của hai lựa chọn này. 
8. Chạy đệ quy bắt đầu từ toàn bộ đoạn`[0, n-1]`với nhân vật`'a'`. 

Quyết định cấu trúc quan trọng nằm ở mọi cấp độ: chúng ta buộc phải chọn chính xác một nửa là hằng số và nửa còn lại hấp thụ đệ quy với ký tự tiếp theo. 

### Tại sao nó hoạt động 

Định nghĩa về một chuỗi tốt thực thi phân tách nhị phân nghiêm ngặt trong đó chính xác một bên của mỗi phần phân tách được “cố định” thành một ký tự ở cấp độ đó. Điều đó có nghĩa là bất kỳ cấu trúc hợp lệ nào đều tương ứng với một chuỗi lựa chọn duy nhất về cố định trái hoặc phải trên tất cả các nút đệ quy. Bằng cách liệt kê cả hai khả năng tại mỗi nút và thêm các giải pháp cấu trúc con tối ưu cho phân đoạn còn lại, chúng tôi bao gồm tất cả các cấu hình hợp lệ. Vì mọi cấu hình hợp lệ sẽ phân tách thành các bài toán con độc lập trên các phân đoạn nhỏ hơn với yêu cầu ký tự tăng dần, nên phép đệ quy sẽ nắm bắt tất cả các khả năng mà không bị chồng chéo hoặc thiếu sót và việc lấy mức tối thiểu sẽ đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case():
    n = int(input().strip())
    s = input().strip()
    
    # prefix frequency per character for fast range mismatch counting
    # pref[i][c] = count of character c in s[:i]
    pref = [[0] * 26 for _ in range(n + 1)]
    
    for i in range(n):
        for c in range(26):
            pref[i + 1][c] = pref[i][c]
        pref[i + 1][ord(s[i]) - 97] += 1
    
    def count_eq(l, r, c):
        total = r - l + 1
        cnt = pref[r + 1][c] - pref[l][c]
        return total - cnt  # mismatches to make all c
    
    from functools import lru_cache
    
    @lru_cache(None)
    def dp(l, r, c):
        if l == r:
            return 0 if ord(s[l]) - 97 == c else 1
        
        mid = (l + r) // 2
        
        cost_left = count_eq(l, mid, c)
        cost_right = count_eq(mid + 1, r, c)
        
        # option 1: left fixed, right recurses
        op1 = cost_left + dp(mid + 1, r, c + 1)
        # option 2: right fixed, left recurses
        op2 = cost_right + dp(l, mid, c + 1)
        
        return min(op1, op2)
    
    return dp(0, n - 1, 0)

t = int(input())
for _ in range(t):
    print(solve_case())
```Việc thực hiện phản ánh đệ quy trực tiếp. Bảng tiền tố được sử dụng để mọi số lượng phân đoạn không khớp đều được tính theo thời gian không đổi cho mỗi truy vấn. Nếu không có nó, mỗi cuộc gọi sẽ quét lại phân đoạn và đẩy độ phức tạp lên một hệ số logarit bổ sung. 

Việc ghi nhớ đảm bảo mỗi`(l, r, c)`trạng thái được tính toán một lần. Từ`c`chỉ bao gồm tối đa 26 chữ cái và cây đệ quy có`O(n)`phân đoạn cho mỗi cấp độ, tổng số trạng thái vẫn có thể quản lý được. 

Một điểm tinh tế là cách xử lý nhân vật`c + 1`. Nó an toàn vì độ sâu đệ quy tối đa là 26 và vấn đề đảm bảo tính khả thi trong các chữ cái viết thường. Việc thực hiện dựa trên các chỉ số nguyên`0..25`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2
ac
```Chúng tôi tính toán`dp(0,1,0)`Ở đâu`0 = 'a'`. 

| Bang (l, r, c) | Chia | Chi phí đồng phục còn lại | Chi phí đồng phục phù hợp | Phương án 1 | Phương án 2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| (0,1,'a') | [0] [1] | 0 / 1 | 1/0 | 0 + dp(1,1,'b') | 1 + dp(0,0,'b') | 1 | 

Ở cấp độ lá,`dp(1,1,'b') = 1`,`dp(0,0,'b') = 1`, vì vậy cả hai lựa chọn đều mang lại kết quả 1. Câu trả lời là 1. 

Điều này cho thấy cách đệ quy thực thi các ràng buộc ký tự ngày càng tăng ngay cả khi chuỗi rất nhỏ. 

### Ví dụ 2 

đầu vào:```
1
4
ceaa
```Chúng tôi tính toán`dp(0,3,'a')`. 

| Tiểu bang | Quyết định | Giải thích | 
| --- | --- | --- | 
| (0,3,'a') | chia | chọn một nửa tốt nhất để trở thành 'a' | 
| giá trái = 3, giá phải = 0 | đã sửa đúng | đúng rồi là "aa" | 
| tái diễn (0,1,'b') | tiếp tục | cấu trúc chuyển sang ký tự tiếp theo | 

Dấu vết này cho thấy rằng khi một nửa đã gần đồng nhất, thuật toán sẽ ưu tiên sửa chữa bên đó một cách tự nhiên và đẩy đệ quy sang nửa còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | mỗi phân đoạn đóng góp công việc liên tục và được ghi nhớ trong 26 ký tự | 
| Không gian | O(n log n) | trạng thái đệ quy cộng với bảng tiền tố | 

Các ràng buộc cho phép tổng số ký tự lên tới 2e5, do đó,`O(n log n)`giải pháp là thoải mái trong giới hạn. Độ sâu đệ quy được giới hạn bởi sự phân tách nhị phân và tiến trình ký tự, ngăn ngừa các vấn đề về ngăn xếp trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_case():
        n = int(input().strip())
        s = input().strip()

        pref = [[0] * 26 for _ in range(n + 1)]
        for i in range(n):
            for c in range(26):
                pref[i + 1][c] = pref[i][c]
            pref[i + 1][ord(s[i]) - 97] += 1

        from functools import lru_cache

        def count_eq(l, r, c):
            return (r - l + 1) - (pref[r + 1][c] - pref[l][c])

        @lru_cache(None)
        def dp(l, r, c):
            if l == r:
                return 0 if ord(s[l]) - 97 == c else 1
            mid = (l + r) // 2
            op1 = count_eq(l, mid, c) + dp(mid + 1, r, c + 1)
            op2 = count_eq(mid + 1, r, c) + dp(l, mid, c + 1)
            return min(op1, op2)

        return dp(0, n - 1, 0)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve_case()))
    return "\n".join(out)

# provided samples
assert run("""6
8
bbdcaaaa
8
asdfghjk
8
ceaaaabb
8
bbaaddcc
1
z
2
ac
""") == """0
7
4
5
1
1"""

# custom cases
assert run("""1
1
a
""") == "0", "single correct char"

assert run("""1
1
b
""") == "1", "single mismatch"

assert run("""1
2
aa
""") == "0", "already good"

assert run("""1
4
bbbb
""") == "3", "all same non-a propagation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn`a`| 0 | tính đúng đắn của trường hợp cơ sở | 
| đơn`b`| 1 | xử lý lá không khớp | 
|`aa`| 0 | cấu trúc không hoạt động | 
|`bbbb`| 3 | tích lũy đệ quy của các thay đổi | 

## Vỏ cạnh 

Chuỗi ký tự đơn kiểm tra đệ quy cơ sở một cách trực tiếp. Đối với đầu vào`z`, hàm đạt`l == r`và trả về 1 vì`'z'`phải được chuyển đổi thành`'a'`. 

Một chuỗi thống nhất như`bbbb`buộc các quyết định đệ quy ở mọi cấp độ. Thuật toán liên tục phân chia và quyết định rằng một bên phải được chuyển thành`'a'`, tích lũy chi phí một cách chính xác vì mỗi cấp đóng góp một số lượng không khớp bằng một nửa kích thước phân khúc trừ đi các chữ cái đúng. 

Một chuỗi có cấu trúc một phần như`acac`chứng tỏ rằng ngay cả khi các nửa cục bộ có vẻ thuận lợi, thì đệ quy vẫn thực thi sự tiến triển của ký tự. Thuật toán đánh giá cả hai hướng ở phần phân chia trên cùng và đảm bảo phía tối ưu được chọn trên toàn bộ thay vì cục bộ.
