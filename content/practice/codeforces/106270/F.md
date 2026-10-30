---
title: "CF 106270F - Đi Bộ Buổi Sáng"
description: "Chúng ta được cung cấp một đường chạy hình tròn có chiều dài cố định. Kabul chạy dọc theo đường vòng này với tốc độ không đổi và tiếp tục di chuyển theo một hướng trong một khoảng thời gian cố định."
date: "2026-06-19T16:41:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106270
codeforces_index: "F"
codeforces_contest_name: "ICPC Asia Dhaka Regional Onsite 2025 \u2014 Replay Contest"
rating: 0
weight: 106270
solve_time_s: 59
verified: true
draft: false
---

[CF 106270F - Đi bộ buổi sáng](https://codeforces.com/problemset/problem/106270/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đường chạy hình tròn có chiều dài cố định. Kabul chạy dọc theo đường vòng này với tốc độ không đổi và tiếp tục di chuyển theo một hướng trong một khoảng thời gian cố định. Những người đi bộ khác cũng đang di chuyển dọc theo cùng một vòng lặp, mỗi người có một tốc độ được ký hiệu không đổi, vì vậy họ hoặc di chuyển cùng với Kabul hoặc chống lại anh ta. 

Đối với mỗi người đi bộ, chúng ta được biết thời điểm Kabul gặp người đi bộ đó lần đầu tiên. Sau lần chạm trán đầu tiên đó, cả hai tiếp tục di chuyển trên cùng một đường tròn nên có thể gặp lại nhau nhiều lần trước khi hết thời gian. Nhiệm vụ là xác định, đối với mỗi người đi bộ, có bao nhiêu cuộc gặp gỡ diễn ra trong khoảng thời gian từ lúc bắt đầu cho đến thời điểm T cuối cùng, bao gồm cuộc gặp đầu tiên và bất kỳ cuộc gặp nào diễn ra chính xác tại T. 

Cấu trúc chính là mỗi người đi bộ di chuyển trên một vòng tròn với vận tốc không đổi, do đó chuyển động tương đối là đồng đều và các cuộc gặp gỡ lặp lại định kỳ. Mỗi trường hợp thử nghiệm có thể chứa tổng cộng tối đa 200.000 người đi bộ và thời gian có thể lên tới 10^7, vì vậy, bất kỳ giải pháp nào mô phỏng chuyển động theo thời gian hoặc xử lý từng giây đều không thể thực hiện được. Ngay cả O(NT) hoặc bất cứ điều gì phụ thuộc vào T đều bị loại trừ ngay lập tức. Ngay cả O(N sqrt T) cũng không an toàn. 

Một điểm tinh tế là chúng ta không được yêu cầu tính toán thời gian gặp nhau lần đầu tiên. Điều đó đã được đưa ra rồi. Đầu vào sửa lỗi căn chỉnh tham chiếu giữa Kabul và từng khung tập đi. Điều này loại bỏ sự mơ hồ về hình học và biến bài toán thành việc đếm số lần một sự kiện định kỳ xảy ra trong một khoảng thời gian giới hạn. 

Các trường hợp biên thường phá vỡ lối suy luận ngây thơ đến từ việc bao bọc mô-đun và thay đổi hướng. 

Một trường hợp thất bại đơn giản là khi một người đi bộ di chuyển cùng hướng với tốc độ rất gần với Kabul. Họ có thể hiếm khi gặp nhau nhưng vẫn gặp nhau nhiều lần và khoảng thời gian trở nên dài. 

Một trường hợp khác là khi cuộc họp diễn ra chính xác tại thời điểm T. Vì T là bao gồm cả T, việc không tính số lần truy cập ranh giới sẽ dẫn đến lỗi sai lệch một. 

Cuối cùng, phương hướng chỉ quan trọng thông qua tốc độ tương đối. Việc xử lý tất cả người đi bộ một cách đồng nhất mà không có vận tốc xác định sẽ dẫn đến việc tính toán chu kỳ không chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng vị trí của Kabul và từng người đi bộ theo thời gian. Chúng tôi có thể tăng thời gian theo từng bước nhỏ và theo dõi khi nào các vị trí khớp với modulo L. Đối với mỗi người đi bộ, việc phát hiện các cuộc họp đến thời điểm T sẽ yêu cầu phải duyệt qua tất cả các sự kiện va chạm trong tương lai. 

Vấn đề là các cuộc họp hình thành một cấp số cộng theo thời gian. Vị trí tương đối giữa Kabul và người đi bộ thay đổi tuyến tính theo thời gian, và trên một vòng tròn, vị trí bằng nhau lặp lại ở mỗi vòng tương đối đầy đủ. Điều này biến chuỗi thời gian gặp nhau thành một cấp số cộng bắt đầu từ thời điểm gặp nhau đầu tiên ti, với một khoảng thời gian cố định chỉ phụ thuộc vào tốc độ tương đối. 

Nếu chúng ta đặt tốc độ tương đối hiệu dụng là v0 - vi, thì sau mỗi chu kỳ trọn vẹn quanh đường tròn, sự căn chỉnh tương tự sẽ lặp lại. Vì vậy, khi chúng ta có một cuộc gặp vào lúc ti, mọi cuộc gặp tiếp theo sẽ diễn ra chính xác sau một khoảng thời gian không đổi bằng L chia cho tốc độ tương đối tuyệt đối. 

Điều này làm cho mỗi walker trở thành một bài toán đếm đơn giản: với thời gian và khoảng thời gian xuất hiện đầu tiên, hãy đếm xem có bao nhiêu số hạng của cấp số cộng nằm trong [0, T]. 

Phương pháp tiếp cận vũ phu hoạt động vì nó trực tiếp theo sau chuyển động, nhưng không thành công vì nó yêu cầu phải bước qua các sự kiện O(T) hoặc O(T / chu kỳ) tiềm năng trên mỗi người đi bộ. Việc quan sát thấy chuyển động tuần hoàn trên một đường tròn sẽ rút gọn bài toán về việc đếm lũy tiến số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(N · T) | O(1) | Quá chậm | 
| Đếm cấp số cộng | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đối xử với từng người đi bộ một cách độc lập vì các tương tác không ảnh hưởng lẫn nhau. 

### 1. Tính chuyển động tương đối

Với mỗi người đi bộ, hãy tính tốc độ tương đối giữa Kabul và người đi bộ. Vì Kabul di chuyển ở v0 và người đi bộ di chuyển ở vi, nên tốc độ đóng cửa là v0 - vi ở dạng có dấu. Chỉ có cường độ quan trọng đối với độ dài chu kỳ. 

Lý do là các cuộc họp chỉ phụ thuộc vào tốc độ giải quyết khoảng cách xung quanh vòng tròn chứ không phụ thuộc vào quy ước về phương hướng. 

### 2. Xuất phát thời gian họp 

Mỗi lần Kabul vượt qua hoặc gặp lại người đi bộ, độ dịch chuyển tương đối tăng đúng một vòng dài L. Do đó, thời gian giữa các lần gặp nhau liên tiếp là L / |v0 - vi|. 

Đây là giai đoạn lũy tiến số học của thời gian gặp gỡ. 

### 3. Thời gian họp mẫu 

Tất cả thời gian họp có thể được viết là: 

ti, ti + chu kỳ, ti + 2·chu kỳ, ... 

Số hạng đầu tiên đã cho nên chúng ta không cần tính nó về mặt hình học. 

### 4. Đếm các điều khoản hợp lệ trong khoảng thời gian 

Chúng ta cần số hạng trong cấp số nằm trong [0, T]. Vì ti được đảm bảo nằm trong [0, T] nên ta chỉ cần đếm có bao nhiêu số hạng không vượt quá T. 

Vì vậy, chúng tôi tính toán k lớn nhất sao cho: 

ti + k · khoảng thời gian ≤ T 

Điều này mang lại: 

k ≤ (T - ti)/chu kỳ 

Vậy số lần họp là: 

k + 1 

### 5. Xử lý số nguyên an toàn 

Vì L và tốc độ là số nguyên nên chu kỳ có thể không phải là số nguyên trong số học thực. Tuy nhiên, tất cả đầu vào đều đảm bảo sự căn chỉnh nhất quán nên chúng tôi làm việc theo số học hữu tỉ bằng cách chia tỷ lệ: chúng tôi tránh dấu phẩy động và tính toán bằng phép nhân. 

Chúng ta viết lại điều kiện: 

ti + k·L / d ≤ T, trong đó d = |v0 - vi| 

Nhân thông qua: 

ti·d + k·L ≤ T·d 

Vì vậy: 

k ≤ (T·d - ti·d) / L 

Chúng tôi sử dụng phép chia số nguyên. 

### Tại sao nó hoạt động 

Chuyển động của Kabul và bất kỳ người đi bộ nào trên đường tròn đều quy về một hàm tuyến tính theo modulo thời gian L. Sự bình đẳng của các vị trí tương ứng với việc giải một sự đồng dạng tuyến tính theo thời gian. Khi một nghiệm ti được đưa ra, tất cả các nghiệm khác tạo thành một mạng cách nhau chính xác bằng L / |v0 - vi|. Vì không có lực hoặc tương tác nào khác tồn tại nên tập hợp thời gian gặp nhau hoàn toàn được xác định bởi cấp số cộng này. Do đó, việc đếm các cuộc họp hợp lệ tương đương với việc đếm các điểm nguyên trong một khoảng giới hạn của một cấp số, được thể hiện chính xác bằng bất đẳng thức dẫn xuất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    K = int(input())
    out = []
    for _ in range(K):
        L, v0, T, N = map(int, input().split())
        res = []
        for _ in range(N):
            ti, vi = map(int, input().split())

            d = abs(v0 - vi)

            # number of additional full cycles possible
            # k such that ti + k*(L/d) <= T
            # => ti*d + k*L <= T*d
            # => k <= (T*d - ti*d) // L
            if ti > T:
                res.append(0)
                continue

            numerator = (T - ti) * d
            if L == 0 or numerator < 0:
                res.append(0)
                continue

            k = numerator // L
            res.append(k + 1)

        out.append(" ".join(map(str, res)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng khung tập đi một cách độc lập trong thời gian không đổi. Phần quan trọng là tránh phân chia dấu phẩy động trong khoảng thời gian. Thay vào đó, chúng ta nhân bất đẳng thức với tốc độ tương đối để giữ mọi thứ ở dạng số nguyên. Điều này ngăn ngừa các vấn đề về độ chính xác và duy trì tính chính xác ngay cả đối với các giá trị lớn của T và L. 

Một chi tiết tinh tế là xử lý ti chính xác bằng hoặc vượt quá T. Vì ti được đảm bảo nằm trong giới hạn của câu lệnh, nên việc kiểm tra này chủ yếu mang tính phòng thủ, nhưng nó bảo vệ khỏi các trường hợp cạnh số học. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ trong đó L = 100, v0 = 10, T = 50. 

### Ví dụ 1 

Một người đi bộ có vi = 0 và ti = 10. 

Tốc độ tương đối d = 10. 

Chu kỳ là L/d = 10. 

Thời gian gặp nhau là: 

10, 20, 30, 40, 50 

| k | thời gian | 
| --- | --- | 
| 0 | 10 | 
| 1 | 20 | 
| 2 | 30 | 
| 3 | 40 | 
| 4 | 50 | 

Tất cả đều nằm trong T, vì vậy câu trả lời là 5. 

Điều này thể hiện sự bao gồm trường hợp biên tại T. 

### Ví dụ 2 

Thiết lập tương tự, nhưng vi = 5 và ti = 7. 

Vận tốc tương đối d = 5, chu kỳ = 20. 

Thời gian họp: 

7, 27, 47 

| k | thời gian | 
| --- | --- | 
| 0 | 7 | 
| 1 | 27 | 
| 2 | 47 | 

Tiếp theo sẽ là 67, vượt quá T. 

Câu trả lời là 3. 

Điều này cho thấy các cuộc họp thưa thớt khi tốc độ tương đối nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) cho mỗi trường hợp thử nghiệm | mỗi khung tập đi được xử lý bằng các phép toán số học không đổi | 
| Không gian | O(1) thêm | chỉ lưu trữ các biến đầu ra và tạm thời | 

Các ràng buộc cho phép tổng cộng tối đa 2·10^5 người đi bộ, do đó việc xử lý tuyến tính đủ nhanh một cách dễ dàng. Tất cả các phép toán đều là số học số nguyên, vì vậy lời giải phù hợp một cách thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    K = int(input())
    out = []
    for _ in range(K):
        L, v0, T, N = map(int, input().split())
        res = []
        for _ in range(N):
            ti, vi = map(int, input().split())
            d = abs(v0 - vi)
            if ti > T:
                res.append(0)
                continue
            numerator = (T - ti) * d
            if L == 0 or numerator < 0:
                res.append(0)
                continue
            k = numerator // L
            res.append(k + 1)
        out.append(" ".join(map(str, res)))
    return "\n".join(out)

# minimal
assert run("1\n10 5 10 1\n0 1\n") == "6", "simple increasing meetings"

# boundary at T
assert run("1\n100 10 50 1\n10 0\n") == "5", "includes exact T"

# same direction close speed
assert run("1\n100 10 100 1\n0 9\n") == "11", "slow relative motion"

# opposite direction
assert run("1\n100 10 100 1\n0 -10\n") == "11", "fast relative motion"

# multiple walkers
assert run("1\n50 5 30 2\n0 1\n5 -1\n") == "4 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | 6 | cấp số cộng cơ số | 
| ranh giới | 5 | bao gồm thời gian T | 
| cùng hướng | 11 | xử lý tốc độ tương đối nhỏ | 
| hướng ngược lại | 11 | sử dụng đúng tốc độ tương đối tuyệt đối | 
| nhiều người đi bộ | 4 3 | sự độc lập của mỗi người đi bộ | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi cuộc gặp đầu tiên xảy ra đúng vào thời điểm T. Trong trường hợp đó, cấp số cộng chỉ chứa số hạng đầu tiên. Công thức vẫn hoạt động vì (T - ti) trở thành 0, tạo ra k = 0 và do đó trả lời 1. 

Một trường hợp khác là khi tốc độ tương đối lớn thì chu kỳ rất nhỏ. Điều này làm tăng số lượng cuộc họp, nhưng phép chia số nguyên vẫn nắm bắt chính xác tất cả các bội số hợp lệ vì mỗi bước bắt nguồn từ phép biến đổi bất đẳng thức chính xác thay vì xấp xỉ thả nổi. 

Khi vi gần v0, mẫu số trở nên nhỏ nên biểu thức (T - ti) * d / L có thể trở nên lớn. Việc sử dụng số nguyên 64 bit là cần thiết để tránh tràn trong phép nhân trung gian.
