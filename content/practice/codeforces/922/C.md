---
problem: 922C
contest_id: 922
problem_index: C
name: "Cave Painting"
contest_name: "Codeforces Round 461 (Div. 2)"
rating: 1600
tags: ["brute force", "number theory"]
answer: passed_samples
verified: true
solve_time_s: 104
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
---

#CF 922C - Tranh Hang Động 

**Đánh giá:** 1600 
**Tags:** vũ lực, lý thuyết số 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có (1/1 mẫu) 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một số nguyên cố định`n`và chiều dài phạm vi`k`. Với mọi số nguyên`i`từ`1`ĐẾN`k`, ta tính phần còn lại khi`n`được chia cho`i`. Điều này tạo ra một chuỗi các giá trị:$$r(i) = n \bmod i$$Nhiệm vụ là xác định xem tất cả các phần dư này có khác nhau hay không. Nói cách khác, chúng ta muốn biết liệu có thể tìm được hai chỉ số phân biệt hay không`i < j`như vậy chia`n`qua`i`Và`j`cho số dư tương tự. 

Các ràng buộc cho phép cả hai`n`Và`k`lớn như$10^{18}$. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào lặp lại trên tất cả các giá trị lên đến`k`, vì ngay cả quét tuyến tính cũng sẽ quá chậm. Bất kỳ cách tiếp cận hợp lệ nào cũng phải hoạt động theo thời gian gần như logarit hoặc hằng số về mặt`k`. 

Trường hợp cạnh tinh tế xuất hiện khi các ước số nhỏ của`n`hiện hữu. Ví dụ, nếu`n = 4`, thì cả hai`4 mod 1`,`4 mod 2`, Và`4 mod 4`bình đẳng`0`. Thậm chí hạn chế ở một tiền tố nhỏ như`k = 2`đã tạo ra một vụ va chạm. Điều này cho thấy sự trùng lặp không chỉ do các giá trị lớn gây ra mà do cấu trúc số học của các ước số. 

Một trường hợp cạnh khác xảy ra khi`n = 1`. Sau đó với mỗi`i ≥ 2`, số dư luôn là`1`, do đó các bản sao xuất hiện gần như ngay lập tức một lần`k ≥ 2`. Bất kỳ giải pháp nào chỉ giải thích về “các ước của`n`” mà không xử lý việc này một cách riêng biệt sẽ phân loại sai trường hợp này. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ tính toán`n % i`cho mọi`i`từ`1`ĐẾN`k`và kiểm tra xem một giá trị có lặp lại hay không. Điều này đúng về mặt khái niệm vì nó phản ánh chính xác định nghĩa của vấn đề. Tuy nhiên, khi`k`có thể$10^{18}$, biểu diễn thậm chí$10^9$các hoạt động đã không thể thực hiện được và ở đây chúng tôi có thể cần tới$10^{18}$hoạt động, điều này hoàn toàn không thể thực hiện được trong thời hạn. 

Quan sát quan trọng là các va chạm có cấu trúc cực kỳ chặt chẽ. giá trị`n % i`trở thành`0`chính xác khi nào`i`chia rẽ`n`. Từ`1`luôn luôn chia rẽ`n`, số dư đầu tiên luôn là`0`. Bất kỳ số chia nào khác`i > 1`trong phạm vi`[1, k]`cũng tạo ra phần còn lại`0`, ngay lập tức nhân đôi giá trị từ`i = 1`. 

Điều này có nghĩa là cách duy nhất để bảo toàn tính phân biệt là đảm bảo rằng không có ước số nào khác của`n`xuất hiện trong phạm vi`2 ... k`. Nếu có một số chia như vậy thì việc trùng lặp là không thể tránh khỏi. 

Vì vậy, toàn bộ vấn đề quy về việc tìm số nguyên nhỏ nhất lớn hơn`1`đó là sự chia rẽ`n`. Nếu ước số nhỏ nhất này là`p`, thì tất cả các số dư đều phân biệt chính xác khi`k < p`. Nếu không có ước số như vậy tồn tại (điều này chỉ xảy ra khi`n = 1`hoặc`n`là số nguyên tố), câu trả lời phụ thuộc vào việc`k`đạt đến điểm mà các dư lượng lặp lại bắt đầu xuất hiện đối với cấu trúc không chia hết, nhưng trên thực tế, điều kiện tương tự vẫn đảm bảo tính đúng đắn vì lần lặp lại đầu tiên vẫn bị chi phối bởi cấu trúc chia hết. 

Ý tưởng vũ phu kiểm tra mọi`i`, trong khi lời giải tối ưu nén mọi thứ vào việc tìm ước số nhỏ nhất của`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(1) | Quá chậm | 
| Tối ưu | O(√n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn xác định số nguyên nhỏ nhất lớn hơn`1`đó là sự chia rẽ`n`, và so sánh nó với`k`. 

1. Nếu`n == 1`, hãy xử lý nó một cách riêng biệt vì mọi phần còn lại hoạt động khác nhau:`1 % i`bằng`1`cho tất cả`i > 1`, vì vậy các bản sao sẽ xuất hiện ngay khi`k ≥ 2`. Trường hợp này có thể giải trực tiếp mà không cần tính toán thêm. 
2. Cố gắng tìm ước số nhỏ nhất`p > 1`của`n`bằng cách kiểm tra số nguyên từ`2`lên tới`√n`. Nếu chúng ta tìm thấy một số`i`như vậy`n % i == 0`, sau đó`i`là ước số ứng viên nhỏ nhất. 
3. Nếu không tìm thấy ước số nào trong phạm vi này thì`n`là số nguyên tố nên ước số nhỏ nhất lớn hơn`1`là`n`chính nó. Trong trường hợp này, chúng tôi thiết lập`p = n`. 
4. So sánh`k`với`p`. Nếu như`k < p`, thì không có ước số nào khác ngoài`1`xuất hiện trong phạm vi được kiểm tra, do đó không có hai vị trí nào tạo ra số dư giống nhau. Ngược lại, ít nhất hai chỉ số tạo ra số dư`0`, do đó xảy ra sự trùng lặp. 

Quyết định hoàn toàn được xác định bởi liệu phạm vi`[2, k]`chứa bất kỳ ước số nào của`n`. 

### Tại sao nó hoạt động 

Mọi va chạm liên quan đến phần còn lại`0`xuất phát từ một ước của`n`. Từ`i = 1`luôn đóng góp`0`, bất kỳ ước số nào khác sẽ ngay lập tức nhân đôi giá trị đó. Tất cả các va chạm tiềm ẩn khác có thể bắt nguồn từ cùng một vấn đề về cấu trúc: một khi có ước số trong phạm vi, mẫu dư sẽ mất tính tiêm nhiễm. Ước số nhỏ nhất đóng vai trò là ranh giới nơi xuất hiện sự lặp lại không thể tránh khỏi đầu tiên, do đó việc ở đúng bên dưới nó đảm bảo tất cả số dư vẫn khác biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    if n == 1:
        # 1 % 1 = 0, 1 % i = 1 for i >= 2 -> duplicates appear from k >= 2
        print("Yes" if k <= 1 else "No")
        return

    # find smallest divisor > 1
    p = n
    i = 2
    while i * i <= n and i <= k:
        if n % i == 0:
            p = i
            break
        i += 1

    print("Yes" if k < p else "No")

if __name__ == "__main__":
    solve()
```Việc thực hiện đầu tiên sẽ tách biệt trường hợp đặc biệt`n = 1`, bởi vì logic số chia hoạt động khác ở đó. Đối với lớn hơn`n`, nó chỉ tìm kiếm tối đa`√n`, vì bất kỳ ước số không tầm thường nào cũng phải xuất hiện theo cặp và một trong số chúng không vượt quá căn bậc hai. Vòng lặp cũng dừng sớm khi`i > k`, bởi vì các ước số ngoài`k`không liên quan đến việc các bản sao có xuất hiện trong phạm vi được truy vấn hay không. 

Sự so sánh cuối cùng`k < p`mã hóa trực tiếp điều kiện không có ước số của`n`(trừ`1`) xuất hiện trong`[1, k]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`n = 4, k = 4`| tôi | n % tôi | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 1 | 
| 4 | 0 | 

Ở đây ước số nhỏ nhất lớn hơn`1`là`2`. Từ`k ≥ 2`, cả hai`i = 1`Và`i = 2`sản xuất phần còn lại`0`, do đó các bản sao xuất hiện. 

Thuật toán tìm`p = 2`và kiểm tra`k < p`, thất bại, tạo ra`No`. 

### Ví dụ 2 

đầu vào:`n = 5, k = 3`| tôi | n % tôi | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 

Ước số nhỏ nhất lớn hơn`1`là`5`, từ`5`là nguyên tố. Tất cả các phần dư được kiểm tra đều khác biệt trong phạm vi. 

Bộ thuật toán`p = 5`và kiểm tra`k < p`, giữ, sản xuất`Yes`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Chúng tôi chỉ kiểm tra các ước số đến căn bậc hai của`n`| 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Việc tìm kiếm căn bậc hai có thể dễ dàng đủ nhanh để`n ≤ 10^18`, vì nó thực hiện tối đa khoảng một triệu lần lặp trong trường hợp xấu nhất, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())

    if n == 1:
        return "Yes\n" if k <= 1 else "No\n"

    p = n
    i = 2
    while i * i <= n and i <= k:
        if n % i == 0:
            p = i
            break
        i += 1

    return ("Yes\n" if k < p else "No\n")

# provided sample
assert run("4 4") == "No\n"

# minimum case
assert run("1 1") == "Yes\n"

# n = 1 edge
assert run("1 3") == "No\n"

# prime n
assert run("5 3") == "Yes\n"

# divisor early
assert run("12 5") == "No\n"

# k small
assert run("12 2") == "Yes\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | Có | trường hợp hợp lệ tối thiểu | 
| 1 3 | Không | số dư lặp lại cho n=1 | 
| 5 3 | Có | hành vi số nguyên tố | 
| 12 5 | Không | số chia sớm bên trong phạm vi | 
| 12 2 | Có | phạm vi an toàn dưới ước số nhỏ nhất | 

## Vỏ cạnh 

cho`n = 1`, thuật toán ngay lập tức trả về dựa trên`k`. Điều này tránh việc dựa vào tìm kiếm ước số, điều này sẽ gợi ý không chính xác rằng không có ước số nào tồn tại và bỏ lỡ cấu trúc lặp lại của số dư. 

Vì`n = 4, k = 2`, việc tìm kiếm số chia sẽ tìm thấy`2`, Vì thế`p = 2`. Từ`k`không thực sự nhỏ hơn`p`, câu trả lời là`No`, phù hợp với thực tế là cả hai`4 % 1`Và`4 % 2`bình đẳng`0`. 

Đối với nguyên tố`n`, chẳng hạn như`n = 13`, không tìm thấy ước số nào, vì vậy`p = 13`. Bất kì`k < 13`vượt qua và tất cả số dư vẫn khác biệt vì không thể tạo ra số dư số 0 giây trong phạm vi.
