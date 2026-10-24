---
title: "CF 105945H - Chuỗi lỏng lẻo"
description: "Chúng ta được cung cấp một chuỗi và được yêu cầu đếm xem chúng ta có thể hình thành bao nhiêu chuỗi con không trống khác nhau dưới sự giới hạn khoảng cách trên các vị trí."
date: "2026-06-22T15:57:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105945
codeforces_index: "H"
codeforces_contest_name: "The 2025 Jiangsu Collegiate Programming Contest, The 2025 Guangdong Provincial Collegiate Programming Contest"
rating: 0
weight: 105945
solve_time_s: 69
verified: true
draft: false
---

[CF 105945H - Chuỗi sau lỏng lẻo](https://codeforces.com/problemset/problem/105945/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi và được yêu cầu đếm xem chúng ta có thể hình thành bao nhiêu chuỗi con không trống khác nhau dưới sự giới hạn khoảng cách trên các vị trí. Một chuỗi con có được bằng cách chọn các ký tự từ chuỗi trong khi vẫn giữ nguyên thứ tự ban đầu của chúng, nhưng chúng ta không được phép chọn các ký tự quá gần nhau trong chuỗi gốc. Chính xác hơn, nếu chúng ta chọn hai vị trí được chọn liên tiếp thì chỉ số của chúng phải khác nhau nhiều hơn$k$. Điều này có nghĩa là mỗi ký tự được chọn ít nhất phải$k+1$vị trí sau ký tự đã chọn trước đó. 

Đầu ra là số chuỗi kết quả riêng biệt trong số tất cả các chuỗi con hợp lệ như vậy, lấy modulo 998244353. 

Tổng kích thước đầu vào lớn, với tổng độ dài chuỗi lên tới$10^6$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào liệt kê các dãy con một cách rõ ràng, vì ngay cả khi không có ràng buộc thì vẫn có nhiều dãy con theo cấp số nhân. Bất kỳ giải pháp nào được chấp nhận đều phải tuyến tính hoặc gần tuyến tính về tổng chiều dài. 

Một quan sát ít rõ ràng hơn là các chữ cái thực tế trong chuỗi không ảnh hưởng đến số lượng các chuỗi con riêng biệt theo ràng buộc này. Hạn chế chỉ phụ thuộc vào vị trí được chọn chứ không phụ thuộc vào giá trị ký tự. Vì mỗi tập hợp vị trí được chọn xác định duy nhất một chuỗi con, nên việc đếm các chuỗi con riêng biệt sẽ tương đương với việc đếm các tập hợp chỉ mục hợp lệ. 

Một trường hợp lỗi phổ biến xuất hiện nếu người ta cố gắng áp dụng chuỗi DP riêng biệt tiêu chuẩn phụ thuộc vào lần xuất hiện cuối cùng của ký tự. Ví dụ: trong một chuỗi như`aaaaa`, suy nghĩ ngây thơ có thể cố gắng hợp nhất các đóng góp theo ký tự, nhưng ràng buộc về khoảng cách sẽ phá vỡ cấu trúc đó. Ràng buộc là vị trí, do đó DP dựa trên ký tự sẽ bị thu gọn không chính xác. 

Một trường hợp cạnh khác phát sinh khi$k = 0$. Khi đó, hai vị trí được chọn liên tiếp bất kỳ phải khác nhau ít nhất 1, điều này không áp đặt hạn chế nào ngoài chính quy tắc thứ tự. Câu trả lời đúng sẽ quy về việc đếm tất cả các dãy con không trống, tức là$2^n - 1$. Bất kỳ sự lặp lại nào vô tình sử dụng ranh giới không chính xác, chẳng hạn như bao gồm vị trí hiện tại trong tổng tiền tố sẽ bị tính quá mức hoặc tạo ra sự tự phụ thuộc. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra tất cả các chuỗi con và kiểm tra xem mỗi chuỗi có thỏa mãn quy tắc giãn cách hay không. Đối với mỗi chuỗi con, chúng tôi xác minh các chỉ số được chọn liền kề theo thời gian tuyến tính. Vì có$2^n$hậu quả, điều này trở nên hoàn toàn không khả thi ngay cả đối với$n = 30$, huống hồ là$10^6$. Thất bại xuất phát từ việc liệt kê các tập hợp con của các vị trí, tăng theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các nhân vật và thay vào đó hãy nghĩ về những lựa chọn hợp lệ của yếu tố cuối cùng. Mỗi dãy con hợp lệ có thể được mô tả duy nhất bởi vị trí được chọn cuối cùng của nó. Nếu chúng ta cố định một vị trí$i$là phần tử cuối cùng, mọi thứ trước nó phải tạo thành một dãy con hợp lệ kết thúc ở vị trí nào đó$j \le i-k-1$. Điều này tạo ra sự lặp lại rõ ràng trên các phạm vi tiền tố thay vì cấu trúc tổ hợp. 

Điều này chuyển vấn đề thành một chương trình động đơn giản trên các vị trí có tổng tiền tố. Chúng tôi duy trì số lượng các chuỗi con hợp lệ kết thúc tại hoặc trước mỗi chỉ mục và sử dụng tổng phạm vi để đảm bảo giới hạn khoảng cách được tôn trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tiền tố DP |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập chỉ mục chuỗi từ 1 đến$n$. Cho phép$dp[i]$biểu thị số dãy con hợp lệ có vị trí được chọn cuối cùng chính xác$i$. Cho phép$pref[i]$là tổng của$dp[1..i]$. 

1. Đối với từng vị trí$i$, hãy xem xét việc bắt đầu một dãy con mới chỉ bao gồm$S[i]$. Điều này đóng góp 1 chuỗi con hợp lệ kết thúc tại$i$. 
2. Bất kỳ dãy con nào dài hơn kết thúc tại$i$phải đến từ một số vị trí kết thúc trước đó$j$, nơi áp dụng quy tắc giãn cách$i - j > k$, tương đương$j \le i - k - 1$. Điều này đảm bảo khoảng trống cuối cùng là hợp lệ và các khoảng trống trước đó đã được các trạng thái trước đó thực thi. 
3. Vì vậy, chúng tôi đặt$dp[i] = 1 + pref[i-k-1]$, điều trị$pref[x] = 0$khi$x \le 0$. 
4. Chúng tôi cập nhật tổng tiền tố:$pref[i] = pref[i-1] + dp[i]$. 
5. Câu trả lời cuối cùng là$pref[n]$, tổng hợp tất cả các chuỗi con hợp lệ kết thúc ở bất kỳ đâu. 

### Tại sao nó hoạt động 

Mỗi dãy con hợp lệ đều có một vị trí được chọn cuối cùng duy nhất$i$. Một lần$i$được cố định, tiền tố còn lại là bất kỳ dãy con hợp lệ nào có vị trí cuối cùng tôn trọng giới hạn khoảng cách với$i$. Phân vùng lặp lại tất cả các chuỗi con hợp lệ theo vị trí cuối cùng này mà không bị trùng lặp. Vì mỗi dãy con được tính chính xác một lần tại chỉ mục cuối của nó nên không xảy ra sự trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()

        dp = [0] * (n + 1)
        pref = [0] * (n + 1)

        for i in range(1, n + 1):
            j = i - k - 1
            if j >= 1:
                dp[i] = (1 + pref[j]) % MOD
            else:
                dp[i] = 1

            pref[i] = (pref[i - 1] + dp[i]) % MOD

        print(pref[n] % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau sự tái diễn. Sự tinh tế duy nhất là xử lý các chỉ số trong đó$i-k-1$trở thành không dương, trong trường hợp đó không có dãy con trước hợp lệ nào được mở rộng. Mảng tiền tố đảm bảo tổng phạm vi được tính theo thời gian không đổi. 

Điểm tinh tế thứ hai là bản thân chuỗi không bao giờ được sử dụng. Mặc dù được cung cấp ở đầu vào nhưng số lượng dãy con hợp lệ chỉ phụ thuộc vào cấu trúc của các lựa chọn chỉ mục được phép chứ không phụ thuộc vào nhận dạng ký tự. Đây là lý do tại sao giải pháp bỏ qua`s`toàn bộ. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ với$n = 5, k = 1$. Chúng tôi tính toán các chuỗi con hợp lệ trong đó các vị trí được chọn phải khác nhau ít nhất là 2. 

| tôi | tính toán dp[i] | dp[i] | trước[i] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 
| 3 | 1 + trước[1] | 2 | 4 | 
| 4 | 1 + trước[2] | 3 | 7 | 
| 5 | 1 + trước[3] | 5 | 12 | 

Câu trả lời cuối cùng là 12. Điều này phản ánh rằng các vị trí sau có nhiều tự do hơn để kết hợp với các chuỗi con hợp lệ trước đó, nhưng chỉ những vị trí đủ xa. 

Bây giờ hãy xem xét$n = 4, k = 2$, trong đó các vị trí phải khác nhau ít nhất là 3. 

| tôi | tính toán dp[i] | dp[i] | trước[i] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 
| 3 | 1 | 1 | 3 | 
| 4 | 1 + trước[1] | 2 | 5 | 

Chỉ vị trí 4 mới có thể mở rộng các chuỗi con trước đó và chỉ những chuỗi kết thúc ở vị trí 1 mới đủ gần để quan trọng. Điều này cho thấy ràng buộc hạn chế phạm vi tương tác một cách hiệu quả như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi vị trí được xử lý một lần với tra cứu tiền tố O(1) | 
| Không gian |$O(n)$| Mảng tính tổng dp và tiền tố | 

Vì tổng của$n$trên tất cả các trường hợp thử nghiệm là$10^6$, giải pháp chạy thoải mái trong giới hạn bằng cách sử dụng một lần tuyến tính duy nhất cho mỗi lần kiểm tra. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()

        dp = [0] * (n + 1)
        pref = [0] * (n + 1)

        for i in range(1, n + 1):
            j = i - k - 1
            if j >= 1:
                dp[i] = (1 + pref[j]) % MOD
            else:
                dp[i] = 1
            pref[i] = (pref[i - 1] + dp[i]) % MOD

        out.append(str(pref[n] % MOD))
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# minimum case
assert run("1\n1 0\na\n") == "1"

# all equal letters, k=0 => 2^n - 1 = 7 for n=3
assert run("1\n3 0\naaa\n") == "7"

# strict spacing
assert run("1\n4 2\nabcd\n") == "5"

# larger simple case
assert run("1\n5 1\nabcde\n") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1,k=0 | 1 | số thứ tự tối thiểu | 
| aaa,k=0 | 7 | tăng trưởng chuỗi con đầy đủ | 
| abcd,k=2 | 5 | hạn chế về khoảng cách mạnh mẽ | 
| abcde,k=1 | 12 | hành vi chồng chéo trung gian | 

## Vỏ cạnh 

Khi nào$k = 0$, sự truy hồi giảm xuống việc đếm chuỗi con cổ điển. Ví dụ với$n = 3$, chúng ta có giá trị dp$1, 2, 4$, cho tổng số 7. Công thức vẫn hoạt động vì$i-k-1 = i-1$, do đó mỗi vị trí sẽ mở rộng chính xác tất cả các chuỗi con trước đó. 

Khi$k \ge n$, không thể chọn hai vị trí cùng nhau. Mỗi mục nhập dp sẽ trở thành 1 vì không thể mở rộng được. Ví dụ với$n = 4, k = 5$, đáp án là 4, tương ứng với việc chọn riêng từng vị trí. 

Đối với rất nhỏ$n$, chẳng hạn như$n = 1$, thuật toán trả về chính xác 1 bất kể$k$, vì chuỗi con duy nhất là ký tự đơn.
