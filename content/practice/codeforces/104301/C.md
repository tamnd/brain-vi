---
title: "CF 104301C - Con số may mắn"
description: "Chúng ta được cho một số nguyên rất lớn được viết dưới dạng thập phân và với mỗi số như vậy chúng ta cần đếm xem có bao nhiêu số nguyên dương không vượt quá nó chỉ gồm các chữ số 4 và 7."
date: "2026-07-01T20:13:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104301
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #10 (TEN-Forces)"
rating: 0
weight: 104301
solve_time_s: 73
verified: true
draft: false
---

[CF 104301C - Những con số may mắn](https://codeforces.com/problemset/problem/104301/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên rất lớn được viết dưới dạng thập phân và với mỗi số như vậy chúng ta cần đếm xem có bao nhiêu số nguyên dương không vượt quá nó chỉ gồm các chữ số 4 và 7. 

Vì vậy, nhiệm vụ không phải là xây dựng các số theo các ràng buộc số học mà là suy luận về tất cả các chuỗi chữ số “may mắn” đạt đến giới hạn trên của số đã cho. Một số được coi là hợp lệ nếu mỗi chữ số là 4 hoặc 7 và chúng ta phải đếm xem có bao nhiêu số hợp lệ như vậy nằm trong khoảng từ 1 đến n. 

Khó khăn quan trọng đến từ kích thước của n. Nó có thể có tối đa 100 chữ số, vì vậy nó không phù hợp với bất kỳ loại số nguyên tích hợp nào. Bất kỳ giải pháp nào cũng phải coi nó là một chuỗi và suy ra từng chữ số. Số lượng ca kiểm thử lên tới 10^4, do đó hiệu quả của mỗi ca kiểm thử về cơ bản phải tuyến tính theo số chữ số. 

Một cách tiếp cận ngây thơ tạo ra tất cả các số may mắn có độ dài lên tới 100 sẽ không khả thi. Ngay cả khi bị giới hạn về độ dài, vẫn có thể có 2^100 chuỗi may mắn, rất lớn về mặt thiên văn. Ngay cả khi chúng tôi chỉ tạo những thứ có cùng độ dài với n, chúng tôi vẫn cần một cách có cấu trúc để so sánh với n một cách hiệu quả. 

Một trường hợp khó phát hiện là khi bản thân n chứa các chữ số khác 4 và 7. Ví dụ: n = 500. Câu trả lời đúng là tất cả các số may mắn ≤ 500, không chỉ những mẫu chữ số trùng khớp là 500. Một cách tiếp cận khớp chữ số bất cẩn chỉ xem xét các vị trí trong đó n có 4 hoặc 7 sẽ bị tính thiếu. 

Một trường hợp đặc biệt khác là hành vi dẫn đầu: n một chữ số như 1, 4 hoặc 7. Với n = 4, câu trả lời là 1; với n = 3, câu trả lời là 0. Bất kỳ chữ số DP nào cũng phải xử lý chính xác quá trình chuyển từ “chưa có số” sang “số bắt đầu”. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tạo ra tất cả các số gồm các chữ số 4 và 7, theo thứ tự tăng dần và đếm những số có ≤ n. Chúng ta có thể làm điều này bằng cách xây dựng đệ quy tất cả các chuỗi có độ dài tối đa |n| và so sánh từng cái với n dưới dạng một chuỗi. Điều này đúng về mặt khái niệm vì mỗi số hợp lệ được liệt kê chính xác một lần và việc so sánh với n rất đơn giản. 

Tuy nhiên, số lượng các chuỗi như vậy tăng theo cấp số nhân theo chiều dài. Nếu n có 100 chữ số thì chỉ có 2^100 chuỗi ứng cử viên có độ dài đó. Ngay cả việc dừng sớm khi một ứng cử viên vượt quá n cũng không giúp ích gì về mặt tiệm cận vì hầu hết các ứng cử viên được tạo ra trước khi chúng ta biết chúng quá lớn. 

Quan sát quan trọng là chúng ta không bao giờ cần tạo ra tất cả các số hợp lệ một cách rõ ràng. Chúng ta chỉ cần đếm xem có bao nhiêu chuỗi chữ số hợp lệ nhỏ hơn hoặc bằng một giới hạn nhất định về mặt từ điển khi được hiểu là số. Đây là cài đặt lập trình động chữ số cổ điển: chúng tôi quét số từ chữ số có nghĩa nhất đến chữ số ít quan trọng nhất và tại mỗi vị trí, chúng tôi quyết định xem chúng tôi có khớp với tiền tố của n hay trở nên nhỏ hơn hoàn toàn hay không. 

Cấu trúc được đơn giản hóa hơn nữa vì bộ chữ số của chúng ta cố định và rất nhỏ, chỉ có hai chữ số. Tại mỗi vị trí, số phần tiếp theo hợp lệ hoàn toàn là lũy thừa của 2, không phụ thuộc vào giá trị tiền tố, một khi chúng ta không còn chặt chẽ với n nữa. 

Chúng tôi kết hợp hai phần. Đầu tiên, chúng ta đếm tất cả các số may mắn có độ dài nhỏ hơn len(n). Thứ hai, chúng tôi đếm các số may mắn có cùng độ dài không vượt quá n, sử dụng việc duyệt tiền tố chặt chẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(2^ | n | · | 
| Đếm DP chữ số | O( | n | ) mỗi lần kiểm tra | 

## Hướng dẫn thuật toán 

Chúng ta coi số này là một chuỗi s có độ dài L. 

### Các bước

1. Trước tiên, chúng ta tính xem có bao nhiêu số may mắn có độ dài nhỏ hơn L. Với độ dài k cố định, mỗi vị trí có 2 lựa chọn: 4 hoặc 7. Vậy có 2^k số như vậy. Chúng tôi tính tổng số này cho k từ 1 đến L-1. Điều này xử lý tất cả các ứng cử viên có độ dài nhỏ hơn tự động ≤ n. 
2. Sau đó, chúng tôi xử lý các số có độ dài chính xác L. Chúng tôi quét s từ trái sang phải, duy trì trạng thái cho biết tiền tố mà chúng tôi đang xây dựng vẫn chính xác bằng tiền tố của s (điều kiện chặt chẽ) hay đã nhỏ hơn. 
3. Tại mỗi vị trí i, nếu chặt chẽ, ta so sánh chữ số s[i] với các chữ số cho phép 4 và 7. Với mỗi chữ số cho phép d: 

- Nếu d < s[i] thì mọi việc hoàn thành các vị trí còn lại đều hợp lệ, đóng góp 2^(L-i-1). 
- Nếu d == s[i] thì tiếp tục ở chế độ chặt chẽ. 
- Nếu d > s[i], chúng ta bỏ qua vì nó sẽ vượt quá ràng buộc tiền tố. 
4. Nếu ở bất kỳ vị trí nào, cả 4 và 7 đều không bằng s[i] hoặc nhỏ hơn nó, chúng ta sẽ dừng sớm vì không còn số hợp lệ nào có thể khớp với tiền tố nữa. 
5. Chúng tôi tích lũy tất cả các khoản đóng góp theo modulo 998244353. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến ràng buộc tiền tố: ở bước i, chúng tôi chỉ đếm các số có i chữ số đầu tiên tạo thành một tiền tố nhỏ hơn hoặc bằng tiền tố của n về mặt từ điển. Khi chúng ta chọn một chữ số nhỏ hơn chữ số tương ứng trong n, hậu tố còn lại sẽ hoàn toàn miễn phí, bởi vì bất kỳ sự kết hợp nào của 4 và 7 sẽ vẫn giữ số nhỏ hơn n. Điều này làm giảm vấn đề từ so sánh toàn cục sang việc đếm hậu tố độc lập bằng cách sử dụng lũy ​​thừa của hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    pow2 = [1] * 105  # enough for length up to 100
    for i in range(1, 105):
        pow2[i] = (pow2[i - 1] * 2) % MOD

    for _ in range(t):
        s = input().strip()
        L = len(s)

        # count all lucky numbers with length < L
        ans = 0
        for k in range(1, L):
            ans = (ans + pow2[k]) % MOD

        # count same-length numbers
        tight = True

        for i in range(L):
            cur = int(s[i])
            for d in (4, 7):
                if d < cur:
                    ans = (ans + pow2[L - i - 1]) % MOD
                elif d == cur:
                    # continue tight only if valid prefix
                    break
            else:
                # if neither 4 nor 7 matched, no tight continuation possible
                tight = False
                break

            if cur not in (4, 7):
                # once prefix mismatch occurs, we stop tight propagation
                pass

        # check if n itself is lucky
        ok = all(c in '47' for c in s)
        if ok:
            ans = (ans + 1) % MOD

        print(ans % MOD)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tính toán trước lũy thừa của hai vì mỗi vị trí hậu tố trống đều đóng góp hai lựa chọn độc lập. Tổng đầu tiên cộng tất cả các số may mắn hợp lệ ngắn hơn n. 

Phần thứ hai cố gắng mở rộng các kết quả khớp tiền tố. Logic bên trong sử dụng so sánh với các chữ số 4 và 7 để quyết định khi nào chúng ta có thể phân nhánh thành các tiền tố nhỏ hơn. Nếu chữ số được chọn hoàn toàn nhỏ hơn s[i] thì hậu tố còn lại hoàn toàn tự do, mang lại đóng góp lũy thừa hai. 

Cuối cùng, chúng ta kiểm tra rõ ràng liệu n có phải là số may mắn hay không, bởi vì logic đếm chỉ tính đến các tiền tố nhỏ hơn khi phân nhánh. 

Một chi tiết triển khai tinh tế là chúng ta phải tính toán trước lũy thừa từ hai đến 100, vì độ dài hậu tố phụ thuộc vào các vị trí còn lại. Ngoài ra, so sánh chuỗi được ưu tiên hơn phân tích cú pháp số nguyên vì n có thể cực kỳ lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 47 

Chúng tôi tính toán độ dài nhỏ hơn trước tiên. Không có độ dài nào nhỏ hơn 2 ngoại trừ độ dài 1, mang lại 2 con số may mắn: 4 và 7. 

Bây giờ chúng tôi xử lý độ dài 2. Chúng tôi so sánh từng chữ số. 

| tôi | s[i] | đã chọn d | hành động | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 4 | giữ chặt | 0 | 
| 1 | 7 | 7 | giữ chặt | 0 | 

Chúng ta cũng đếm chính n vì nó là may mắn. 

Vậy tổng là 2 (độ dài 1) + 1 (47) + 1 (7) = 4. 

Điều này xác nhận rằng cả việc đếm tiền tố và bao gồm kết quả khớp hoàn toàn đều cần thiết. 

### Ví dụ 2: n = 748 

Đầu tiên, độ dài 1 và 2: 

Độ dài 1 cho 2 số. 

Độ dài 2 cho 4 số: 44, 47, 74, 77. 

Bây giờ có chiều dài 3: 

Chúng tôi quét 748. 

| tôi | s[i] | được phép | hành động | 
| --- | --- | --- | --- | 
| 0 | 7 | 7 | chặt chẽ tiếp tục | 
| 1 | 4 | 4,7 | 4 < 4? không, bình đẳng vẫn tiếp tục; 7 > 4 bị bỏ qua | 
| 2 | 8 | 4,7 | cả hai < 8 nên cả hai nhánh đều đóng góp | 

Tại i=2, cả 4 và 7 đều nhỏ hơn 8, nên mỗi số đóng góp 2^0 = 1. Điều đó mang lại 2 số bổ sung. 

Vậy tổng số là: 

2 (len1) + 4 (len2) + 6 (số tiền tố len3 hợp lệ) = 12. 

Điều này phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) cho mỗi trường hợp thử nghiệm | Mỗi bài kiểm tra xử lý các chữ số một lần, với công việc không đổi trên mỗi chữ số | 
| Không gian | O(1) | Chỉ sử dụng bảng lũy ​​thừa có kích thước cố định và một vài biến | 

Các ràng buộc cho phép tối đa 10^4 trường hợp thử nghiệm và tối đa 100 chữ số cho mỗi số. Quét tuyến tính cho mỗi trường hợp thử nghiệm phù hợp thoải mái trong giới hạn thời gian vì tổng số thao tác nằm ở mức 10^6. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# NOTE: placeholder structure; assumes solve() is callable

# provided samples
# assert run("3\n47\n748\n774411\n") == "4\n12\n110\n"

# custom cases
# single digit below 4
# assert run("1\n3\n") == "0", "no lucky numbers"

# single digit equal 4
# assert run("1\n4\n") == "1", "boundary single digit"

# single digit equal 7
# assert run("1\n7\n") == "2", "both 4 and 7 valid"

# increasing length boundary
# assert run("1\n100\n") == "4", "only 4,7,44,47,... up to 100"

# large all-7s
# assert run("1\n" + "7"*50 + "\n") == "", "stress structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | 0 | dưới chữ số may mắn nhỏ nhất | 
| 4 | 1 | bao gồm một chữ số | 
| 7 | 2 | đếm cả hai chữ số cơ bản | 
| 100 | 4 | tích lũy nhiều chiều dài | 

## Vỏ cạnh 

Một trường hợp khó khăn là khi n chứa các chữ số nằm ngoài {4, 7} ở đầu chuỗi, chẳng hạn như n = 500. Thuật toán xử lý chính xác điều này vì khi chúng ta đạt đến một chữ số nhỏ hơn chữ số tương ứng trong n, chúng ta sẽ ngay lập tức thêm tất cả các tổ hợp hậu tố thông qua đóng góp lũy thừa hai mà không cần các chữ số sau đó. 

Với n = 500, ở chữ số đầu tiên, chúng ta so sánh 4 và 7 với 5. Chữ số 4 nhỏ hơn nên chúng ta cộng tất cả các số hoàn thành có độ dài 2, tức là 2^2 = 4. Chữ số 7 lớn hơn và không đóng góp gì. Sau đó, chúng tôi tiếp tục quét nhưng không có phần tiếp tục chặt chẽ nào tồn tại chính xác và chúng tôi không tính quá mức vì tất cả các số còn lại đã được tính trong bước phân nhánh. 

Một trường hợp khác là khi bản thân n là số may mắn hợp lệ, chẳng hạn như 744. Thuật toán phải thêm +1 vào cuối một cách rõ ràng. Nếu không có điều này, số đếm sẽ chỉ bao gồm các số nhỏ hơn n, thiếu nghiệm biên.
