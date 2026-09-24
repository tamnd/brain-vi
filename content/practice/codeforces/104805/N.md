---
title: "CF 104805N - Lời đầu tiên"
description: "Chúng tôi được đưa cho một cuốn từ điển nhỏ gồm những từ mà Veronica biết, và sau đó là một chuỗi dài thể hiện những gì Igor đã viết ra dưới dạng đoạn độc thoại của cô ấy."
date: "2026-06-28T13:22:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104805
codeforces_index: "N"
codeforces_contest_name: "Central Russia Regional Contest, 2022"
rating: 0
weight: 104805
solve_time_s: 58
verified: true
draft: false
---

[CF 104805N - Lời đầu tiên](https://codeforces.com/problemset/problem/104805/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một cuốn từ điển nhỏ gồm những từ mà Veronica biết, và sau đó là một chuỗi dài thể hiện những gì Igor đã viết ra dưới dạng đoạn độc thoại của cô ấy. Nhiệm vụ là quyết định xem chuỗi dài có thể được tạo thành bằng cách nối một số chuỗi các từ đã biết mà không cần chèn hoặc xóa các ký tự hay không. 

Nói cách khác, chúng ta muốn biết liệu chúng ta có thể chia chuỗi`s`thành một chuỗi các chuỗi con liền kề nhau, trong đó mỗi chuỗi con chính xác là một trong các từ đã cho. Cùng một từ có thể được sử dụng lại nhiều lần và chúng ta không cần sử dụng tất cả các từ, chỉ cần bao phủ chính xác toàn bộ chuỗi. 

Kích thước đầu vào cho phép tối đa 100 từ đã biết, với tổng độ dài kết hợp tối đa là 100.000 và chuỗi độc thoại`s`cũng có thể dài tới 100.000 ký tự. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các phân đoạn có thể có của chuỗi theo thời gian hàm mũ. Một giải pháp quét liên tục`s`hiệu quả hoặc thực hiện lập trình động trên các vị trí trong`s`, là cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi các từ trùng lặp hoặc chia sẻ tiền tố. Ví dụ: nếu từ điển chứa`"a"`,`"aa"`, Và`"aaa"`, và chuỗi là`"aaaaa"`, cách tiếp cận tham lam ngây thơ có thể thất bại do chọn các trận đấu ngắn quá sớm. Một vấn đề khác phát sinh nếu chúng ta cố gắng so khớp các từ ở mọi vị trí mà không lập chỉ mục, điều này có thể dẫn đến việc quét toàn bộ lặp đi lặp lại tất cả các từ cho mọi vị trí ký tự. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là bắt đầu từ chỉ mục`0`TRONG`s`và thử đệ quy từng từ trong từ điển khớp với tiền tố hiện tại, sau đó tiếp tục từ cuối kết quả khớp đó. Đây là tìm kiếm theo chiều sâu đơn giản trên các vị trí trong chuỗi. 

Mặc dù đúng nhưng cách tiếp cận này có thể lặp lại các bài toán con tương tự nhiều lần. Từ một chỉ số nhất định trong`s`, chúng tôi có thể thử khớp tất cả các từ nhiều lần. Trong trường hợp xấu nhất, mỗi vị trí phân nhánh thành tối đa 100 lựa chọn và mỗi kết quả phù hợp có giá lên tới O (độ dài của từ), dẫn đến hành vi theo cấp số nhân đối với đầu vào đối nghịch. 

Quan sát quan trọng là vấn đề chỉ phụ thuộc vào chỉ số hiện tại trong`s`, không phải về cách chúng tôi đến đó. Điều này gợi ý một công thức lập trình động: cho mỗi vị trí`i`, chúng tôi muốn biết liệu hậu tố`s[i:]`có thể được phân đoạn đầy đủ. Sau khi tính toán, kết quả này sẽ được sử dụng lại. 

Chúng ta có thể tăng tốc việc so khớp bằng cách lặp lại các từ và kiểm tra xem`s[i:i+len(word)]`tương đương với từ đó. Vì tổng chiều dài từ bị giới hạn nên việc so sánh trực tiếp này đủ hiệu quả. Ngoài ra, người ta có thể xây dựng một bộ ba, nhưng với những hạn chế, việc quét từ điển nhỏ cho mỗi vị trí là đủ. 

Giải pháp cuối cùng trở thành DP trên các vị trí trong`s`, trong đó chúng tôi cố gắng mở rộng các trạng thái hợp lệ về phía trước bằng cách sử dụng tất cả các từ trong từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS | O(exp) | O(n) | Quá chậm | 
| DP qua các vị trí | O(n * Total_words) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một mảng boolean`dp`, Ở đâu`dp[i]`cho biết liệu tiền tố`s[0:i]`có thể được phân chia thành các từ đã biết. 

1. Khởi tạo`dp[0] = True`, vì tiền tố trống luôn hợp lệ. Mọi vị trí khác đều bắt đầu bằng Sai. 
2. Lặp lại các vị trí`i`từ`0`ĐẾN`len(s)`. 
3. Nếu`dp[i]`là Sai, bỏ qua nó. Điều này có nghĩa là không có vị trí phân đoạn hợp lệ`i`, vì vậy việc mở rộng từ nó là vô ích. 
4. Đối với mỗi từ đã biết`w`, kiểm tra xem chuỗi con bắt đầu tại`i`trận đấu`w`, tức là,`s[i:i+len(w)] == w`. 
5. Nếu khớp thì đặt`dp[i + len(w)] = True`, bởi vì chúng tôi có thể mở rộng phân đoạn hợp lệ đến điểm cuối đó. 
6. Sau khi xử lý xong tất cả các vị trí, hãy kiểm tra`dp[len(s)]`. Nếu đúng thì xuất ra “YES”, nếu không thì xuất ra “NO”. 

### Tại sao nó hoạt động 

Tại mọi chỉ số`i`,`dp[i]`thể hiện chính xác liệu có tồn tại một phân đoạn hợp lệ của tiền tố lên đến`i`. Khi mở rộng việc sử dụng một từ, chúng tôi chỉ nối thêm các từ trong từ điển hợp lệ, vì vậy mọi trạng thái mới được đánh dấu đều tương ứng với một phân đoạn hợp lệ. Ngược lại, bất kỳ phân đoạn hợp lệ nào cũng phải kết thúc bằng một số từ trong từ điển và quá trình chuyển đổi cuối cùng sẽ đánh dấu điểm cuối tương ứng. Điều này đảm bảo không có cấu trúc hợp lệ nào bị bỏ sót và không có cấu trúc không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    words = [input().strip() for _ in range(n)]
    s = input().strip()

    L = len(s)
    dp = [False] * (L + 1)
    dp[0] = True

    for i in range(L):
        if not dp[i]:
            continue

        for w in words:
            lw = len(w)
            if i + lw <= L and s[i:i + lw] == w:
                dp[i + lw] = True

    print("YES" if dp[L] else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau công thức DP. Vòng lặp bên ngoài lặp lại các vị trí trong chuỗi và các chuyển đổi chỉ được thực hiện từ các vị trí có thể truy cập được, giúp tránh được những công việc không cần thiết. So sánh chuỗi con`s[i:i+lw] == w`là an toàn vì chúng tôi kiểm tra giới hạn một cách rõ ràng trước khi cắt. 

Một lỗi phổ biến ở đây là quên hạn chế chuyển tiếp tới các phạm vi có thể truy cập`dp[i] == True`, điều này sẽ cho phép tạo các phân đoạn từ tiền tố không hợp lệ một cách không chính xác. Một vấn đề nhỏ khác là không kiểm tra giới hạn trước khi cắt, điều này có thể dẫn đến kết quả khớp một phần không chính xác hoặc lãng phí công việc. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng mẫu được cung cấp. 

đầu vào:```
4
bububu
mama
papa
matan
bububumatanbububumama
```Chúng tôi theo dõi`dp[i]`chỉ ở những vị trí có liên quan. 

| tôi | dp[i] | từ phù hợp | vị trí cập nhật | 
| --- | --- | --- | --- | 
| 0 | Đúng | bububu | dp[6] = Đúng | 
| 0 | Đúng | không ai khác | - | 
| 6 | Đúng | matan | dp[11] = Đúng | 
| 11 | Đúng | bububu | dp[17] = Đúng | 
| 17 | Đúng | mẹ | dp[21] = Đúng | 

Cuối cùng,`dp[21] = True`, vì vậy câu trả lời là CÓ. 

Dấu vết này cho thấy nhiều từ trong từ điển có thể được xâu chuỗi và DP tích lũy chính xác các ranh giới có thể tiếp cận. 

Chúng ta cũng có thể xem xét một trường hợp thất bại: 

đầu vào:```
2
ab
aba
abab
```Tại`i = 0`, cả hai`"ab"`Và`"aba"`là những chuyển tiếp có thể xảy ra. DP đánh dấu vị trí 2 và 3. Từ 2, ta đạt 4 thông qua`"ab"`, vì vậy có thể bảo hiểm đầy đủ. Điều này chứng tỏ tại sao việc khám phá tất cả các quá trình chuyển đổi là cần thiết chứ không chỉ là một sự lựa chọn tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m * k) | n là độ dài của s, m là số từ, k là độ dài từ trung bình do so sánh chuỗi con | 
| Không gian | O(n) | mảng dp trên các vị trí chuỗi | 

Với n ≤ 100.000 và m ≤ 100, sản phẩm vẫn được chấp nhận trong Python vì mỗi lần chuyển đổi là một so sánh chuỗi con giới hạn đơn giản và việc bỏ qua sớm các trạng thái không thể truy cập sẽ làm giảm đáng kể công việc trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve_and_capture()

def solve_and_capture():
    import sys
    input = sys.stdin.readline

    n = int(input())
    words = [input().strip() for _ in range(n)]
    s = input().strip()

    L = len(s)
    dp = [False] * (L + 1)
    dp[0] = True

    for i in range(L):
        if not dp[i]:
            continue
        for w in words:
            lw = len(w)
            if i + lw <= L and s[i:i+lw] == w:
                dp[i+lw] = True

    return "YES" if dp[L] else "NO"

# provided sample
assert run("""4
bububu
mama
papa
matan
bububumatanbububumama
""") == "YES"

# single word exact match
assert run("""1
abc
abc
""") == "YES"

# impossible case
assert run("""2
a
b
abx
""") == "NO"

# repeated concatenation
assert run("""2
ab
aba
abab
""") == "YES"

# long chain
assert run("""3
a
aa
aaa
aaaaaa
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khớp từ đơn | CÓ | chấp nhận cơ bản | 
| chuỗi không thể | KHÔNG | trường hợp từ chối | 
| nối lặp đi lặp lại | CÓ | phân nhánh đúng đắn | 
| từ chồng chéo | CÓ | chuyển đổi không tham lam | 

## Vỏ cạnh 

Một trường hợp quan trọng là các từ trong từ điển chồng chéo trong đó các lựa chọn tham lam không thành công. Coi như`words = ["a", "aa"]`Và`s = "aaa"`. Bộ thuật toán`dp[1]`,`dp[2]`, Và`dp[3]`chính xác vì nó khám phá cả hai tiện ích mở rộng ở mỗi chỉ mục có thể truy cập. Một cách tiếp cận tham lam có thể mất`"aa"`đầu tiên và bị mắc kẹt, nhưng DP vẫn giữ`"a"`như một con đường thay thế. 

Một trường hợp khác là khi nhiều từ khớp ở cùng một vị trí. Vì các quá trình chuyển đổi có tính bổ sung nên việc đánh dấu nhiều điểm cuối đảm bảo không có phân đoạn hợp lệ nào bị mất. 

Cuối cùng, khả năng tiếp cận trống được xử lý rõ ràng: nếu`dp[i]`không bao giờ đạt được, không có quá trình chuyển đổi nào bắt nguồn từ đó, ngăn cản việc truyền bá không hợp lệ sang các trạng thái sau này.
