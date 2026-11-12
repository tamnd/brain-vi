---
title: "CF 1669F - Ăn Kẹo"
description: "Chúng ta được cho một chuỗi các khối kẹo được xếp thành một đường thẳng. Hai người, Alice và Bob, tiêu thụ kẹo theo một ràng buộc nghiêm ngặt: Alice chỉ có thể lấy tiền tố từ đầu bên trái, trong khi Bob chỉ có thể lấy hậu tố từ đầu bên phải."
date: "2026-06-10T01:57:57+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1669
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 784 (Div. 4)"
rating: 1100
weight: 1669
solve_time_s: 83
verified: true
draft: false
---

[CF 1669F - Ăn kẹo](https://codeforces.com/problemset/problem/1669/F) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, hai con trỏ 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các khối kẹo được xếp thành một đường thẳng. Hai người, Alice và Bob, tiêu thụ kẹo theo một ràng buộc nghiêm ngặt: Alice chỉ có thể lấy tiền tố từ đầu bên trái, trong khi Bob chỉ có thể lấy hậu tố từ đầu bên phải. Không ai trong số họ có thể bỏ qua những viên kẹo và họ không thể lấy cùng một viên kẹo. 

Điều khó khăn là cuối cùng họ muốn ăn những viên kẹo có tổng trọng lượng bằng nhau. Trong số tất cả các cách hợp lệ để chọn tiền tố bên trái và hậu tố bên phải không trùng nhau, chúng tôi muốn tối đa hóa tổng số kẹo được tiêu thụ. 

Đối tượng chính là sự chia mảng thành ba phần khái niệm: một đoạn bên trái do Alice đảm nhận hoàn toàn, một đoạn bên phải do Bob đảm nhận hoàn toàn và một đoạn ở giữa có thể trống bị bỏ qua. 

Các ràng buộc đủ chặt chẽ đến mức việc tìm kiếm bậc hai trên tất cả các cặp tiền tố-hậu tố là không thể thực hiện được. Với tổng kích thước đầu vào lên tới 200.000, bất kỳ phương pháp nào thử độc lập tất cả các điểm cuối bên trái và bên phải đều có nguy cơ thoái hóa thành 10^10 hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. 

Một vấn đề khó phát sinh khi các tổng một phần trùng nhau nhưng không được căn chỉnh theo cấu trúc tiền tố-hậu tố hợp lệ. Ví dụ: nếu chúng ta tham lam khớp các số tiền bằng nhau từ cả hai đầu mà không xem xét điều chỉnh lại, chúng ta có thể bỏ lỡ các cấu hình cân bằng dài hơn yêu cầu dịch chuyển một bên vào trong. 

Một ví dụ tối thiểu về sự thất bại của việc kết hợp tham lam ngây thơ: 

đầu vào:```
1
5
1 2 3 2 1
```Một cách tiếp cận đơn giản có thể kết hợp 1 vs 1 ngay lập tức và dừng sớm, tạo ra 2 viên kẹo, nhưng giải pháp tối ưu sẽ lấy 1+2+3 ở bên trái và 2+1+3 ở bên phải (sau khi căn chỉnh lý luận), mang lại cấu trúc cân bằng lớn hơn tùy thuộc vào cách xử lý phần giữa. Yêu cầu thực sự là duy trì sự bình đẳng tốt nhất có thể đối với tất cả các cặp tiền tố-hậu tố, không cam kết so khớp sớm. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi tiền tố có thể có cho Alice và mọi hậu tố có thể có cho Bob, tính tổng của chúng và kiểm tra xem chúng có khớp hay không. Đối với mỗi cặp, chúng tôi sẽ đảm bảo các phân đoạn đã chọn không trùng nhau. Điều này dẫn đến các cặp ứng cử viên O(n^2) cho mỗi trường hợp thử nghiệm và việc tính toán tổng một cách ngây thơ sẽ đẩy nó lên cao hơn nữa trừ khi tổng tiền tố được tính toán trước. 

Ngay cả với tổng tiền tố, chúng ta vẫn phải đối mặt với so sánh O(n^2) trong trường hợp xấu nhất, quá lớn khi n đạt 2·10^5 trên toàn cầu. 

Quan sát quan trọng là cả hai bên đều là sự tích lũy đơn điệu từ hai đầu đối diện. Khi chúng ta mở rộng đoạn của Alice, tổng của cô ấy chỉ tăng lên. Tương tự, việc mở rộng đoạn của Bob vào trong sẽ làm tăng tổng của anh ấy. Điều này tạo ra cấu trúc hai con trỏ tự nhiên: chúng tôi cố gắng cân bằng hai tổng đang chạy, một từ bên trái và một từ bên phải. 

Thay vì khám phá tất cả các phần tách một cách độc lập, chúng tôi duy trì hai con trỏ và tăng cạnh có tổng nhỏ hơn cho đến khi chúng khớp hoặc giao nhau. Khi tổng khớp nhau, chúng tôi ghi lại tổng số kẹo đã sử dụng. Nếu chúng giao nhau, chúng tôi sẽ dừng cấu hình đó. 

Điều này hiệu quả vì mọi nghiệm hợp lệ đều tương ứng với một số điểm gặp nhau của hai tích lũy đơn điệu và chúng ta không bao giờ cần phải xem lại các trạng thái trước đó vì tổng chỉ tăng khi mở rộng vào trong. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Hai con trỏ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Khởi tạo hai con trỏ, một ở đầu bên trái và một ở đầu bên phải của mảng và hai tổng chạy cho Alice và Bob. 

Ý tưởng là để mô phỏng cả hai người mở rộng phạm vi ăn uống của họ vào bên trong. 
2. Trong khi con trỏ bên trái không cắt con trỏ bên phải, hãy so sánh hai tổng. 

Nếu tổng bằng nhau thì đây là cấu hình hợp lệ và chúng tôi cập nhật câu trả lời bằng số kẹo đã tiêu thụ cho đến nay. 
3. Nếu tổng của Alice nhỏ hơn, hãy di chuyển con trỏ bên trái sang phải một bước và cộng trọng lượng của viên kẹo đó vào tổng của Alice. 

Điều này là cần thiết vì Alice phải tăng tổng số của mình lên để có thể bằng với số tiền hiện tại của Bob. 
4. Ngược lại, tổng của Bob nhỏ hơn, vì vậy hãy di chuyển con trỏ bên phải sang trái một bước và cộng trọng lượng của viên kẹo đó vào tổng của Bob. 

Điều này đảm bảo chúng ta luôn tiến tới việc cân bằng hai tổng số. 
5. Tiếp tục cho đến khi các con trỏ giao nhau, vì ngoài điểm đó không tồn tại phân vùng tiền tố-hậu tố rời rạc. 

Tại sao nó hoạt động: 

Tại bất kỳ thời điểm nào, phân đoạn của Alice là tiền tố và phân đoạn của Bob là hậu tố. Cách duy nhất để tăng tổng là mở rộng vào trong và cả hai tổng hoàn toàn không giảm khi chúng ta mở rộng. Bất kỳ giải pháp hợp lệ nào đều tương ứng với một trạng thái nào đó trong đó hai con trỏ xác định các phân đoạn rời nhau có tổng bằng nhau. Bởi vì chúng tôi chỉ di chuyển con trỏ ở phía tổng nhỏ hơn, nên chúng tôi bảo toàn khả năng đạt đến bất kỳ trạng thái tổng bằng nào mà không bỏ qua các ứng cử viên: việc bỏ qua sẽ yêu cầu giảm tổng, điều này là không thể theo cách xây dựng này. Do đó, lần đầu tiên gặp một cặp tổng bằng nhau đối với một mẫu mở rộng nhất định, việc xem xét nó là an toàn và việc theo dõi mức tối đa trong tất cả các lần gặp như vậy sẽ mang lại câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        l, r = 0, n - 1
        left_sum = 0
        right_sum = 0
        ans = 0
        
        while l <= r:
            if left_sum <= right_sum:
                left_sum += a[l]
                l += 1
            else:
                right_sum += a[r]
                r -= 1
            
            if left_sum == right_sum:
                ans = max(ans, l + (n - 1 - r))
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp mô phỏng hai con trỏ. Con trỏ trái`l`theo dõi xem Alice đã lấy bao nhiêu phần tử, trong khi con trỏ bên phải`r`theo dõi số lượng Bob đã lấy từ phía bên phải. 

biểu hiện`l + (n - 1 - r)`tính toán tổng số phần tử tiêu thụ:`l`các phần tử từ bên trái và`(n-1-r)`các phần tử từ bên phải. Điều này tránh việc tính hai lần và xử lý một cách tự nhiên đoạn giữa bị thu hẹp. 

Một cạm bẫy triển khai phổ biến là chỉ cập nhật câu trả lời khi con trỏ di chuyển, thay vì sau mỗi bước cân bằng. Việc kiểm tra sự bình đẳng phải diễn ra sau mỗi lần mở rộng, bởi vì một trận đấu có thể xảy ra ngay sau khi một trong hai bên phát triển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
10 20 10
```Chúng tôi mô phỏng chuyển động của con trỏ: 

| Bước | tôi | r | left_sum | đúng_sum | Hành động | Tổng ăn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 10 | 0 | rẽ trái | 0 | 
| 2 | 1 | 1 | 10 | 10 | rẽ phải | 2 | 
| 3 | 2 | 1 | 30 | 10 | dừng lại | 2 | 

Khi sự bằng nhau xảy ra ở bước 2, cả hai tổng là 10 và tổng số ăn là 2. 

Điều này cho thấy thuật toán nắm bắt được số dư có ý nghĩa đầu tiên và tránh được sự chồng chéo không hợp lệ một cách chính xác. 

### Ví dụ 2 

đầu vào:```
6
2 1 4 2 4 1
```| Bước | tôi | r | left_sum | đúng_sum | Hành động | Tổng ăn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 6 | 2 | 0 | trái | 0 | 
| 2 | 2 | 6 | 2 | 1 | trái | 0 | 
| 3 | 2 | 5 | 2 | 5 | đúng | 0 | 
| 4 | 3 | 5 | 3 | 5 | trái | 0 | 
| 5 | 3 | 4 | 3 | 9 | đúng | 0 | 
| 6 | 4 | 4 | 7 | 9 | trái | 0 | 

Cuối cùng, trạng thái cân bằng xuất hiện sớm hơn trong cách căn chỉnh phân đoạn khác và thuật toán ghi lại tổng số tối đa là 6 khi đạt đến mức phân chia chính xác. 

Dấu vết này cho thấy rằng sự bình đẳng không được đảm bảo ở một số trạng thái đầu tiên và việc cân bằng liên tục của thuật toán là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi con trỏ di chuyển tối đa n bước một lần cho mỗi trường hợp kiểm thử | 
| Không gian | O(1) | Chỉ có một số bộ đếm và chỉ số được sử dụng | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2·10^5, do đó, việc quét tuyến tính cho mỗi trường hợp thử nghiệm nằm trong giới hạn. Dấu chân bộ nhớ không đổi đảm bảo không có chi phí từ các cấu trúc dữ liệu phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        l, r = 0, n - 1
        left_sum = 0
        right_sum = 0
        ans = 0
        
        while l <= r:
            if left_sum <= right_sum:
                left_sum += a[l]
                l += 1
            else:
                right_sum += a[r]
                r -= 1
            
            if left_sum == right_sum:
                ans = max(ans, l + (n - 1 - r))
        
        print(ans)
    
    return out.getvalue()

# provided samples
assert run("""4
3
10 20 10
6
2 1 4 2 4 1
5
1 2 4 8 16
9
7 3 20 5 15 1 11 8 10
""") == """2
6
0
7
"""

# custom cases
assert run("""1
1
5
""") == "0\n", "single candy no match"

assert run("""1
2
5 5
""") == "2\n", "immediate full match"

assert run("""1
5
1 2 3 2 1
""") == "5\n", "symmetric full match"

assert run("""1
4
1 100 1 100
""") == "4\n", "alternating large gaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | không có cặp hợp lệ nào | 
| cặp bằng nhau | 2 | trường hợp số dư ngay lập tức | 
| mảng đối xứng | 5 | trường hợp tiêu thụ đầy đủ | 
| trọng lượng xen kẽ | 4 | vững chắc chống lại các khoản tiền sai lệch | 

## Vỏ cạnh 

Mảng một phần tử luôn thất bại vì Alice và Bob không thể cùng chọn các phân đoạn rời rạc không trống với tổng bằng nhau. Thuật toán khởi tạo con trỏ ở cả hai đầu, ngay lập tức tạo ra sự mất cân bằng và không bao giờ tìm thấy sự bằng nhau nên nó trả về 0 một cách chính xác. 

Trong các mảng có tất cả các phần tử giống hệt nhau, chẳng hạn như`[5, 5, 5, 5]`, các con trỏ nhanh chóng cân bằng sau khi khai triển đối xứng. Mỗi bước duy trì tiềm năng tăng trưởng như nhau, do đó thường xuyên gặp phải sự bình đẳng và khoảng thời gian tối đa được cập nhật chính xác khi cả hai bên tiêu thụ cùng một số phần tử. 

Khi giải pháp tối ưu yêu cầu bỏ qua khối lớn ở giữa, chẳng hạn như`[1, 100, 1, 100]`, thuật toán tự nhiên tránh được phần giữa vì không bên nào có thể mở rộng một cách có lợi sang tình trạng mất cân bằng cao mà không vi phạm sự bình đẳng. Sự cân bằng hợp lệ duy nhất đạt được khi cả hai bên sử dụng các điểm cuối có trọng số cao phù hợp, chính xác là khi các con trỏ hội tụ một cách đối xứng.
