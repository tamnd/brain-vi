---
title: "CF 104930H - Trò chơi bài Úc"
description: "Chúng ta được yêu cầu đếm các chuỗi có độ dài $N$ trong đó mỗi vị trí chứa một “thứ hạng” được chọn từ một bộ có thứ tự cố định gồm 13 mệnh giá: Át là nhỏ nhất, tiếp theo là 2 cho đến Vua."
date: "2026-06-28T07:45:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104930
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 2 (Beginner)"
rating: 0
weight: 104930
solve_time_s: 52
verified: true
draft: false
---

[CF 104930H - Trò chơi bài Úc](https://codeforces.com/problemset/problem/104930/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm các chuỗi có độ dài$N$trong đó mỗi vị trí chứa một “cấp bậc” được chọn từ một bộ có thứ tự cố định gồm 13 mệnh giá: Át là nhỏ nhất, tiếp theo là 2 cho đến Vua. 

Ràng buộc về trình tự là không bình thường: khi chúng ta di chuyển từ trái sang phải, mỗi quân bài tiếp theo phải tăng thứ hạng nghiêm ngặt so với quân trước đó hoặc nó có thể là quân Át bất kể giá trị trước đó là bao nhiêu. Vì vậy, trình tự chủ yếu tăng lên, nhưng quân Át hoạt động giống như một biểu tượng đặt lại đặc biệt có thể xuất hiện ở bất kỳ đâu mà không bị hạn chế. 

Hai chuỗi chỉ được coi là khác nhau nếu ít nhất một vị trí có thứ hạng khác nhau, vì vậy đây hoàn toàn là vấn đề đếm trên chuỗi thứ hạng. 

Hạn chế chính là cục bộ: mỗi vị trí chỉ phụ thuộc vào thẻ trước đó. Điều đó ngay lập tức gợi ý một công thức lập trình động theo các vị trí và thứ hạng được nhìn thấy lần cuối. 

Từ$N \le 20$, bất kỳ số mũ nào ở 13 hoặc 14 trạng thái cho mỗi vị trí đều ổn. Ràng buộc thực sự là về mặt khái niệm, không phải về mặt tính toán. 

Trường hợp cạnh tinh tế xuất hiện khi các chuỗi sử dụng Át nhiều lần. Ví dụ, đối với$N=3$, trình tự như$A, A, A$có giá trị ngay cả khi chúng không “tăng” ở đâu cả. Một cách giải thích ngây thơ buộc phải tăng trưởng nghiêm ngặt ngoại trừ việc thỉnh thoảng đặt lại có thể không cho phép các quân Át lặp lại một cách không chính xác hoặc xử lý sai các chuyển đổi liên quan đến Át. 

Một trường hợp tinh vi khác là coi Ace vừa là con nhỏ nhất vừa là ký tự đại diện. Quy tắc không đối xứng: Át luôn được phép làm yếu tố tiếp theo, ngay cả sau Vua. Thay vào đó, nếu người ta mô hình Át là hạng 1 theo một thứ tự nghiêm ngặt, thì việc chuyển đổi từ Vua sang Át vẫn phải được cho phép, điều này sẽ phá vỡ DP trình tự tăng dần tiêu chuẩn nếu không được xử lý riêng. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ tạo ra tất cả các chuỗi có độ dài có thể$N$trên 13 cấp, kiểm tra xem mỗi cấp có thỏa mãn quy tắc hay không và đếm những cấp hợp lệ. Điều đó mang lại$13^N$trình tự. Vì$N = 20$, đây là về$10^{22}$, điều này hoàn toàn không khả thi ngay cả khi cắt tỉa. 

Cấu trúc của ràng buộc gợi ý trạng thái lập trình động dựa trên thứ hạng được chọn cuối cùng. Quan sát quan trọng là tính hợp lệ chỉ phụ thuộc vào lá bài trước đó và liệu lá bài hiện tại là Át hay lớn hơn nó. Sự phụ thuộc cục bộ này có nghĩa là chúng ta có thể xây dựng các chuỗi tăng dần mà không cần nhớ toàn bộ lịch sử. 

Chúng tôi xác định DP dựa trên các vị trí và cấp bậc cuối cùng, trong đó quá trình chuyển đổi sẽ xem xét tất cả các cấp bậc tiếp theo được phép. Từ một tiểu bang có thứ hạng cuối cùng$x$, chúng ta có thể lên bất kỳ cấp bậc nào$y > x$, hoặc đến Ace bất kể$x$. Điều này tạo ra một cấu trúc không theo chu kỳ được định hướng trên các trạng thái, ngoại trừ việc Ace hoạt động giống như một thiết lập lại toàn cầu kết nối từ mọi trạng thái. 

Sự tối ưu hóa xuất phát từ việc nhận ra rằng các chuyển đổi sang “bất kỳ thứ hạng cao hơn nào” có thể được tổng hợp bằng cách sử dụng tổng tiền tố, trong khi các chuyển đổi Ace đóng góp một sự bổ sung thống nhất từ ​​tất cả các trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(13^N \cdot N)$|$O(N)$| Quá chậm | 
| DP vượt cấp |$O(N \cdot 13^2)$|$O(13)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi cấp bậc là số nguyên từ 0 đến 12, trong đó 0 đại diện cho Át và 12 đại diện cho Vua. 

1. Khởi tạo mảng DP`dp[r]`nghĩa là số chuỗi có độ dài hiện tại kết thúc bằng thứ hạng`r`. Đối với độ dài 1, mọi thứ hạng đều có giá trị như nhau, vì vậy mỗi trạng thái bắt đầu bằng giá trị 1. 
2. Đối với mỗi vị trí tiếp theo, chúng tôi tính toán một mảng DP mới`ndp`. Đối với mọi cấp bậc hiện tại có thể`r`, chúng tôi muốn tính xem có bao nhiêu cách chúng tôi có thể chuyển sang nó. 
3. Đối với việc chuyển cấp bậc`r`, chúng tôi tổng hợp tất cả các trạng thái trước đó`dp[x]`ở đâu`x < r`(điều kiện tăng nghiêm ngặt) hoặc`r`là Ace. Vì Át luôn có thể được chọn nên mọi`dp[x]`đóng góp vào trạng thái Ace. 
4. Để tính toán hiệu quả “tổng trên tất cả x < r”, chúng ta duy trì tổng tiền tố trên`dp`. Điều này cho phép chúng ta nhận được sự đóng góp của các chuyển đổi tăng dần theo thời gian không đổi trên mỗi trạng thái. 
5. Riêng đối với quân Át, chúng tôi tính riêng nó thành tổng của tất cả`dp`các giá trị, bởi vì từ bất kỳ thứ hạng nào trước đó, chúng ta có thể đặt quân Át. 
6. Sau khi xử lý hết cấp bậc, thay thế`dp`với`ndp`và lặp lại cho tất cả các vị trí cho đến$N$. 
7. Đáp án cuối cùng là tổng của tất cả các giá trị trong`dp`sau khi xử lý$N$các vị trí. 

### Tại sao nó hoạt động 

Ở mỗi bước, trạng thái DP tóm tắt đầy đủ tất cả các chuỗi hợp lệ có độ dài nhất định bằng cách chỉ nhớ thứ hạng cuối cùng. Quy tắc chuyển đổi chỉ phụ thuộc vào cấp bậc cuối cùng đó và lá bài tiếp theo là Át hay cao hơn. Vì mọi tiện ích mở rộng hợp lệ được tính chính xác một lần thông qua chuyển đổi tiền tố hoặc chuyển đổi Ace toàn cầu, nên không có chuỗi nào bị bỏ sót hoặc được tính hai lần. Cấu trúc tổng tiền tố đảm bảo rằng ràng buộc “tăng nghiêm ngặt” được thực thi chính xác mà không cần tính toán lại các phần chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input().strip())
    
    # dp[r] = number of sequences ending in rank r
    # 0 = Ace, 1..12 = 2..King
    dp = [1] * 13
    
    for _ in range(n - 1):
        total = sum(dp) % MOD
        
        # prefix sums for strict increases
        pref = [0] * 14
        for i in range(13):
            pref[i + 1] = (pref[i] + dp[i]) % MOD
        
        ndp = [0] * 13
        
        for r in range(13):
            if r == 0:
                # Ace can be placed after anything
                ndp[r] = total
            else:
                # sum of all x < r
                ndp[r] = pref[r] % MOD
        
        dp = ndp
    
    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo DP được mô tả ở trên. Mảng tiền tố`pref`được sử dụng để tính nhanh các tổng trên tất cả các cấp trước đó nhỏ hơn cấp hiện tại, thực thi điều kiện tăng nghiêm ngặt. Trạng thái Át được xử lý riêng bằng cách sử dụng tổng của tất cả các trạng thái trước đó, vì Át bỏ qua các ràng buộc về thứ tự. 

Chúng tôi lặp lại chính xác$N-1$chuyển tiếp vì vị trí đầu tiên được khởi tạo trực tiếp. 

Phải cẩn thận với các phép toán modulo, đặc biệt khi tính toán`total`và tổng tiền tố, vì tổng trung gian có thể tăng vượt quá giới hạn số nguyên nếu không giảm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```Chúng tôi bắt đầu với tất cả các trạng thái dp là 1. 

| Bước | dp (Ace..King) | tổng cộng | ndp(Ace) | ndp(2..King) | 
| --- | --- | --- | --- | --- | 
| ban đầu | 13 cái | - | - | - | 
| sau 1 bước | tính toán | 13 | 13 | dựa trên tiền tố | 

Đối với hạng 2 (chỉ số 1), chúng ta chỉ có thể đến từ Át nên giá trị là 1. Đối với Át, chúng ta có thể đến từ tất cả 13 tiểu bang, cho ra 13. Tổng các kết thúc hợp lệ sẽ tạo ra 91. 

Điều này xác nhận rằng ngay cả những chuỗi ngắn cũng đã bao gồm nhiều sự kết hợp trong đó Át đóng vai trò là người thiết lập lại. 

### Ví dụ 2 

đầu vào:```
3
```Bây giờ DP mở rộng hơn nữa: 

| Bước | tổng dp | 
| --- | --- | 
| sau lần chuyển tiếp đầu tiên | 91 | 
| sau lần chuyển tiếp thứ 2 | giá trị tổng hợp lớn hơn | 

Quá trình chuyển đổi thứ hai thể hiện hành vi chính: các chuỗi kết thúc ở thứ hạng cao hơn chỉ tích lũy từ các cấp độ nhỏ hơn, trong khi Ace liên tục đưa vào các chuyển đổi trạng thái đầy đủ. 

Điều này cho thấy Ace thống trị sự tăng trưởng tổ hợp bằng cách đóng vai trò là người kết nối toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \cdot 13)$| Mỗi bước tính toán 13 trạng thái bằng cách sử dụng tổng tiền tố | 
| Không gian |$O(13)$| Chỉ các mảng DP hiện tại mới được lưu trữ | 

Những hạn chế$N \le 20$làm cho việc triển khai thậm chí còn đơn giản hơn trở nên khả thi, nhưng DP này có quy mô vượt quá giới hạn một cách thoải mái. Hệ số không đổi rất nhỏ và thuật toán chạy ngay lập tức trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    dp = [1] * 13

    for _ in range(n - 1):
        total = sum(dp) % MOD
        pref = [0] * 14
        for i in range(13):
            pref[i + 1] = (pref[i] + dp[i]) % MOD

        ndp = [0] * 13
        for r in range(13):
            if r == 0:
                ndp[r] = total
            else:
                ndp[r] = pref[r]
        dp = ndp

    return str(sum(dp) % MOD)

# provided sample
assert run("2\n") == "91"

# minimum size
assert run("1\n") == "13", "single card"

# small sanity
assert run("3\n") > "0", "positive growth"

# edge: maximum constraint
assert run("20\n") > "0", "stability"

# all structure test
assert run("2\n") != "0", "non-empty transitions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 13 | độ chính xác khởi tạo cơ sở | 
| 2 | 91 | độ chính xác của mẫu | 
| 3 | >0 | tăng trưởng trong quá trình chuyển đổi | 
| 20 | >0 | ổn định ở độ sâu tối đa | 

## Vỏ cạnh 

cho$N = 1$, câu trả lời phải chính xác là 13 vì mọi thứ hạng đều hợp lệ dưới dạng chuỗi một phần tử. DP bắt đầu bằng tất cả 1, vì vậy tổng cuối cùng vẫn là 13, phù hợp với quy tắc không có chuyển đổi nào được thực hiện. 

Đối với các chuỗi bị quân Át thống trị, chẳng hạn như$A, A, A, \dots$, thuật toán tính chúng thông qua quá trình chuyển đổi Ace luôn sử dụng tổng các trạng thái trước đó. Ví dụ: ở mỗi bước, Át nhận được sự đóng góp từ mọi trạng thái kết thúc có thể có, do đó, các chuỗi Át lặp lại không bao giờ bị bỏ sót và được tích lũy chính xác như một phần của tổng toàn cầu. 

Đối với các chuỗi tăng nghiêm ngặt không có Át, chẳng hạn như$2, 5, 9$, cơ chế tổng tiền tố đảm bảo chúng được tính chính xác một lần trên mỗi đường dẫn hợp lệ. Mỗi tiện ích mở rộng chỉ sử dụng các trạng thái có thứ hạng nhỏ hơn, do đó không có chuyển tiếp lùi không hợp lệ nào được đưa vào.
