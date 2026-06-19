---
title: "CF 1005D - Polycarp và Phân 3"
description: "Chúng ta được cung cấp một chuỗi thập phân rất dài và chúng ta được phép chèn các phần cắt giữa các chữ số liền kề, chia nó thành các phần liền kề nhau."
date: "2026-06-16T23:20:02+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1005
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 496 (Div. 3)"
rating: 1500
weight: 1005
solve_time_s: 72
verified: true
draft: false
---

[CF 1005D - Polycarp và Div 3](https://codeforces.com/problemset/problem/1005/D) 

**Đánh giá:** 1500 
**Tags:** dp, tham lam, lý thuyết số 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi thập phân rất dài và chúng ta được phép chèn các phần cắt giữa các chữ số liền kề, chia nó thành các phần liền kề nhau. Mỗi đoạn được hiểu là một số, nhưng có một hạn chế: các đoạn có số 0 đứng đầu không được phép trừ khi đoạn đó chính xác là một chữ số`"0"`. 

Sau khi tách, chúng tôi đếm xem có bao nhiêu phần đại diện cho các số chia hết cho 3 và chúng tôi muốn tối đa hóa số lượng này. 

Vì vậy, nhiệm vụ không phải là tính giá trị của chính số đó mà là chọn một phân vùng theo chuỗi chữ số của nó. Mỗi quân đóng góp 1 điểm nếu tổng các chữ số của nó chia hết cho 3 hoặc 0 nếu ngược lại. Mục tiêu là tối đa hóa số lượng phân khúc “tốt”. 

Độ dài đầu vào có thể lên tới 200.000 chữ số. Điều này ngay lập tức loại trừ bất kỳ chiến lược nào thử tất cả các phân vùng. Một phân vùng được xác định bởi 199.999 quyết định cắt nhị phân, do đó, việc ép buộc brute sẽ theo cấp số nhân trong trường hợp xấu nhất. 

Việc lập trình động đơn giản trên tất cả các phân đoạn cũng sẽ không khả thi nếu nó đánh giá lại các tổng chuỗi con nhiều lần, vì có các chuỗi con O(n^2). 

Một điểm tinh tế quan trọng là các số 0 đứng đầu chỉ được phép trong các phân đoạn một ký tự. Điều này quan trọng vì nó ngăn cản chúng ta tự do nhóm các số 0 thành các khối dài hơn để thao tác tổng. 

Một số tình huống khó khăn rất quan trọng cần ghi nhớ. 

Ví dụ: nếu chuỗi toàn số 0`"0000"`, câu trả lời tối ưu là 4 vì mỗi chữ số phải được cách ly thành`"0"`, và mỗi số đều chia hết cho 3. 

Nếu chuỗi có các chữ số như`"111111"`, chiến lược tốt nhất là chỉ chia càng nhiều càng tốt nếu nó giúp cân bằng mod 3 tổng, nhưng việc chia tùy ý không phải lúc nào cũng tối ưu vì việc nhóm ảnh hưởng đến cấu trúc chia hết. 

Nếu chuỗi là`"303"`, lấy toàn bộ chuỗi sẽ có tổng 6, vì vậy câu trả lời là 1, nhưng chia thành`"3|0|3"`đưa ra 3 phân đoạn hợp lệ. Điều này cho thấy khả năng chia hết không đơn điệu khi sáp nhập. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là xem xét mọi cách có thể để cắt chuỗi, sau đó đánh giá từng phân vùng kết quả. Đối với một chuỗi có độ dài n, có 2^(n−1) cách đặt các vết cắt. Ngay cả khi việc đánh giá một phân vùng mất O(n), tổng số sẽ trở thành O(n·2^n), điều này là không thể đối với n lên tới 200.000. 

Một lực lượng vũ phu có cấu trúc hơn là lập trình động. Đặt dp[i] là số lượng phân đoạn hợp lệ tối đa trong tiền tố i. Từ i, chúng tôi thử tất cả các vị trí cắt trước đó j < i và kiểm tra xem chuỗi con s[j..i] có chia hết cho 3 hay không. Việc kiểm tra tính chia hết có thể được thực hiện thông qua các tổng tiền tố, vì vậy mỗi lần chuyển đổi là O(1), nhưng DP là O(n^2), vẫn quá lớn. 

Quan sát quan trọng là khả năng chia hết cho 3 chỉ phụ thuộc vào tổng các chữ số theo modulo 3. Nếu một đoạn có tổng các chữ số bằng 0 mod 3 thì nó đóng góp +1. 

Bây giờ chúng ta viết lại bài toán dưới dạng tổng tiền tố modulo 3. Đặt pref[i] là tổng các chữ số lên tới i modulo 3. Một phân đoạn (j+1..i) là phù hợp nếu pref[i] == pref[j]. 

Vì vậy, chúng tôi muốn phân vùng mảng thành nhiều phân đoạn nhất có thể sao cho mỗi phân đoạn được chọn tương ứng với hai trạng thái mod tiền tố bằng nhau. 

Điều này chuyển vấn đề thành việc chọn một chuỗi các vị trí có mod bằng nhau, nhưng với ràng buộc là các phân đoạn rời rạc và liền kề nhau. 

Cái nhìn sâu sắc tham lam là bất cứ khi nào chúng ta nhìn thấy một vị trí i, chúng ta nên cố gắng đóng một phân đoạn ở vị trí j trước đó sớm nhất với cùng trạng thái mod tiền tố vẫn hợp lệ. Điều này tương đương với việc ghép nối các trạng thái tiền tố bằng nhau theo cách tối đa hóa số lượng cặp, đồng thời tôn trọng thứ tự. 

Cấu trúc tối ưu trở thành một cuộc quét tham lam với ba bộ đếm theo dõi số lần mỗi lớp modulo tiền tố “có sẵn” làm điểm bắt đầu cho một phân đoạn. 

Ràng buộc số 0 đứng đầu hóa ra không ảnh hưởng đến số lượng tối ưu vì các số 0 có một chữ số luôn là các phân đoạn hợp lệ và có thể được coi là các chữ số bình thường có giá trị 0. 

Chúng tôi tham lam duy trì số lượng trạng thái tiền tố hiện đang mở và bất cứ khi nào chúng tôi thấy lại trạng thái phù hợp, chúng tôi sẽ đóng một phân đoạn ngay lập tức để tối đa hóa tính linh hoạt trong tương lai. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các phân vùng | O(2^n · n) | O(n) | Quá chậm | 
| DP trên chuỗi con | O(n^2) | O(n) | Quá chậm | 
| Tiền tố mod + ghép nối tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển đổi chuỗi chữ số thành một chuỗi các tổng tiền tố modulo 3. 

1. Tính toán các giá trị modulo tiền tố trong khi quét chuỗi từ trái sang phải. Tại vị trí i, chúng ta biết số dư r của tiền tố hiện tại. 
2. Duy trì bộ đếm cnt[r] biểu thị số lần phần dư này xuất hiện dưới dạng “ranh giới bắt đầu” cho một phân đoạn không được đóng. Ban đầu cnt[0] = 1 vì tiền tố trống trước chuỗi đóng vai trò là điểm bắt đầu. 
3. Khi ở vị trí i, chúng ta kiểm tra số dư r hiện tại. Nếu cnt[r] dương, chúng ta lập tức tạo thành một đoạn kết thúc tại i. Chúng tôi tăng câu trả lời và giảm cnt[r] vì chúng tôi đang ghép vị trí này với vị trí trước đó. 
4. Nếu cnt[r] bằng 0, chúng tôi lưu trữ phần dư này làm điểm bắt đầu tiềm năng trong tương lai bằng cách tăng cnt[r]. 
5. Tiếp tục cho đến hết chuỗi. 

Ý tưởng chính là mỗi khi chúng tôi đóng một phân đoạn, chúng tôi đảm bảo tổng các chữ số của nó chia hết cho 3 vì cả hai điểm cuối đều có chung tiền tố tổng modulo 3. 

### Tại sao nó hoạt động 

Thuật toán về cơ bản là ghép các phần tiền tố bằng nhau trong quá trình quét từ trái sang phải trong khi luôn đóng phân đoạn hợp lệ sớm nhất có thể. Bất kỳ giải pháp tối ưu nào cũng tạo ra sự ghép nối giữa các vị trí có giá trị modulo tiền tố bằng nhau. Nếu chúng tôi trì hoãn việc đóng một phân đoạn khi có thể thì chúng tôi chỉ làm giảm tính linh hoạt trong tương lai vì việc mở đó có thể được sử dụng để tạo thành một phân đoạn hợp lệ khác sau này. Do đó, việc đóng cửa một cách tham lam ngay lập tức không bao giờ làm giảm số lượng các cặp có thể có và tổng số phân đoạn bằng số lượng các cặp như vậy.

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    
    cnt = [0, 0, 0]
    cnt[0] = 1
    
    pref = 0
    ans = 0
    
    for ch in s:
        pref = (pref + (ord(ch) - 48)) % 3
        
        if cnt[pref] > 0:
            ans += 1
            cnt = [0, 0, 0]
            cnt[0] = 1
            pref = 0
        else:
            cnt[pref] += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ lại phần tiền tố hiện tại còn lại và một mảng tần số nhỏ. Bước đặt lại tương ứng với việc cắt chuỗi: khi chúng ta tạo thành một phân đoạn hợp lệ, chúng ta sẽ bắt đầu đếm lại từ ranh giới đó. Điều này rất quan trọng vì các phân đoạn rời rạc nên logic tiền tố phải khởi động lại sau mỗi lần cắt. 

Điểm tinh tế là đặt lại cả trạng thái bộ đếm và phần tiền tố còn lại sau khi cắt thành công. Nếu không đặt lại, chúng tôi sẽ cho phép các phân đoạn chồng chéo một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`3121`Chúng tôi tính toán tiền tố mod 3 và trạng thái theo dõi. 

| tôi | chữ số | tiền tố tổng hợp mod 3 | cnt[pref] trước | hành động | phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | vâng | đóng đoạn | 1 | 
| khởi động lại | 1 | 1 | không | trạng thái mở | 1 | 
| 2 | 2 | 0 | không | trạng thái mở | 1 | 
| 3 | 1 | 1 | vâng | đóng đoạn | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này cho thấy việc đóng cửa sớm làm tăng tổng số lượng như thế nào. 

### Ví dụ 2:`303`| tôi | chữ số | tiền tố mod 3 | cnt[pref] | hành động | phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | vâng | đóng | 1 | 
| khởi động lại | 0 | 0 | vâng | đóng | 2 | 
| khởi động lại | 3 | 0 | vâng | đóng | 3 | 

Câu trả lời là 3, đạt được bằng cách chia thành các chữ số đơn. 

Điều này xác nhận rằng các số 0 và bội số của 3 lặp lại có thể tạo thành các phân đoạn độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét một lần các chữ số với công việc liên tục trên mỗi bước | 
| Không gian | O(1) | chỉ có ba bộ đếm và trạng thái tiền tố | 

Thuật toán xử lý thoải mái tới 200.000 chữ số trong giới hạn vì nó tránh được mọi quá trình xử lý lồng nhau hoặc đánh giá chuỗi con. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    cnt = [0, 0, 0]
    cnt[0] = 1
    pref = 0
    ans = 0

    for ch in s:
        pref = (pref + (ord(ch) - 48)) % 3
        if cnt[pref] > 0:
            ans += 1
            cnt = [0, 0, 0]
            cnt[0] = 1
            pref = 0
        else:
            cnt[pref] += 1

    return str(ans)

# provided sample
assert run("3121\n") == "2", "sample 1"

# all digits form one number divisible by 3
assert run("6\n") == "1", "single digit divisible"

# all zeros
assert run("0000\n") == "4", "each zero is a segment"

# alternating pattern
assert run("303\n") == "3", "max splitting"

# no divisible segments except single digits
assert run("111\n") == "1", "minimal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 | 1 | xử lý chia hết một chữ số | 
| 0000 | 4 | ràng buộc phân chia bằng không | 
| 303 | 3 | sự chia tách tối đa tham lam | 
| 111 | 1 | không có sự phân chia có lợi | 

## Vỏ cạnh 

cho`"0000"`, thuật toán bắt đầu với cnt[0] = 1. Ở chữ số đầu tiên, tiền tố là 0 nên một đoạn sẽ bị đóng ngay lập tức. Trạng thái đặt lại và lặp lại, tạo ra bốn phân đoạn. Điều này phù hợp với ràng buộc rằng mỗi số 0 phải được cách ly. 

Vì`"111"`, các giá trị mod tiền tố là 1, 2, 0. Chỉ vị trí cuối cùng mới đóng một phân đoạn, do đó chỉ có một phân đoạn được hình thành. Thuật toán không buộc phải phân chia không cần thiết vì cnt[r] chỉ được sử dụng khi tồn tại trạng thái khớp. 

Vì`"303"`, các số 0 tiền tố lặp lại cho phép đóng ngay lập tức ở mỗi bước, tạo ra sự phân đoạn tối đa. Việc thiết lập lại đảm bảo mỗi lần cắt là độc lập và ngăn chặn việc tái sử dụng trạng thái tiền tố chồng chéo.
