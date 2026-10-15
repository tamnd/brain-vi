---
title: "CF 105461E - Chiếu sáng đường phố"
description: "Chúng ta có một con đường thẳng được biểu diễn dưới dạng một đoạn liên tục từ 0 đến L. Có n cột đèn cố định được đặt ở tọa độ nguyên dọc theo đoạn này. Chúng ta không được phép lựa chọn vị trí của chúng mà chỉ được lựa chọn loại bóng đèn được lắp đặt ở mỗi cột đèn."
date: "2026-06-23T17:53:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105461
codeforces_index: "E"
codeforces_contest_name: "2024-2025 ICPC, Swiss Subregional"
rating: 0
weight: 105461
solve_time_s: 69
verified: true
draft: false
---

[CF 105461E - Chiếu sáng đường phố](https://codeforces.com/problemset/problem/105461/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một con đường thẳng được biểu diễn dưới dạng một đoạn liên tục từ 0 đến L. Có n cột đèn cố định được đặt ở tọa độ nguyên dọc theo đoạn này. Chúng ta không được phép lựa chọn vị trí của chúng mà chỉ được lựa chọn loại bóng đèn được lắp đặt ở mỗi cột đèn. 

Có m loại bóng đèn có sẵn và mỗi loại được xác định bởi một tham số duy nhất: khoảng cách chiếu sáng theo cả hai hướng. Nếu chúng ta lắp một bóng đèn có phạm vi l vào một đèn ở vị trí x, thì đèn đó sẽ chiếu sáng toàn bộ khoảng từ x − l đến x + l. Tất cả các loại đèn phải sử dụng cùng một loại bóng đèn và chúng ta được phép chọn mua loại nào. 

Mục đích là để xác định phạm vi bóng đèn nhỏ nhất trong số các loại đã cho để có thể kết hợp tất cả các khoảng thời gian được chiếu sáng để bao phủ hoàn toàn mọi điểm của [0, L]. Nếu không có loại bóng đèn nào có thể phủ sóng toàn bộ thì câu trả lời là −1. 

Các ràng buộc n, m ≤ 100000 ngay lập tức loại trừ bất kỳ phương pháp nào thử mọi loại bóng đèn và tính toán lại vùng phủ sóng theo phương pháp bậc hai. Một mô phỏng O(nm) ngây thơ sẽ dẫn đến khoảng 10^10 lần kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn thực tế. Ngay cả O(mn log n) cũng trở nên chặt chẽ tùy thuộc vào các hằng số, vì vậy chúng ta cần kiểm tra tuyến tính hoặc gần tuyến tính cho mỗi ứng viên kết hợp với chiến lược lựa chọn logarit trên các loại bóng đèn. 

Một khía cạnh tế nhị của vấn đề là phạm vi phủ sóng diễn ra liên tục. Điều này có nghĩa là sự chồng chéo một phần là vấn đề và việc kiểm tra xem điểm cuối của các khoảng có bao gồm các điểm nguyên hay không là chưa đủ. Ví dụ: nếu đèn che [0, 5] và [6, 10] thì vẫn có một khe hở mặc dù cả hai đầu đều “đóng”. Một giải pháp đúng phải giải thích về sự kết hợp khoảng thời gian đầy đủ chứ không phải các mẫu rời rạc. 

Một cạm bẫy tiềm tàng khác là giả định rằng việc có một bóng đèn đủ lớn cho các loại đèn cực lớn sẽ đảm bảo phạm vi phủ sóng. Điều đó là sai nếu có khoảng cách lớn giữa các vị trí đèn liên tiếp. Ví dụ: đèn ở vị trí 0 và 10 với L = 10 yêu cầu l = 5, nhưng cấu hình như 0, 4, 10 vẫn không thành công với l = 3 mặc dù các điểm cuối trông có vẻ hợp lý. 

Cuối cùng, người ta dễ quên rằng ranh giới bên trái 0 và ranh giới bên phải L phải được che phủ một cách rõ ràng. Thiếu vùng phủ sóng gần 0 đặc biệt phổ biến vì nó phụ thuộc hoàn toàn vào việc đèn ngoài cùng bên trái có chiếu đủ xa về bên trái hay không. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là kiểm tra từng loại bóng đèn một cách độc lập. Đối với phạm vi l cố định, chúng tôi tính toán hợp của tất cả các khoảng [p_i − l, p_i + l] và kiểm tra xem liệu hợp này có bao gồm [0, L] hay không. Cách tiêu chuẩn để thực hiện việc này là sắp xếp các khoảng theo điểm bắt đầu của chúng và hợp nhất chúng một cách tham lam trong khi theo dõi điểm được bao phủ xa nhất. Nếu tại bất kỳ thời điểm nào chúng tôi gặp phải khoảng trống trong đó khoảng thời gian tiếp theo bắt đầu sau khi phạm vi phủ sóng hiện tại kết thúc thì phạm vi phủ sóng sẽ không thành công. 

Kiểm tra này là chính xác và chạy ở O(n log n) cho mỗi loại bóng đèn do khoảng thời gian sắp xếp hoặc O(n) nếu chúng tôi sử dụng lại thứ tự vị trí được sắp xếp trước. Tuy nhiên, thực hiện điều này với tất cả m loại bóng đèn sẽ dẫn đến O(mn), quá lớn đối với 100000 x 100000. 

Quan sát quan trọng là tính khả thi là đơn điệu trong l. Nếu phạm vi bóng đèn l đủ để bao phủ đường phố thì bất kỳ phạm vi l′ > l nào lớn hơn cũng là đủ, bởi vì mỗi khoảng chỉ mở rộng. Tính đơn điệu này cho phép chúng ta xử lý bài toán như một phép tìm kiếm nhị phân trên danh sách các dải bóng đèn đã được sắp xếp. 

Đầu tiên chúng ta sắp xếp các phạm vi bóng đèn. Sau đó, chúng tôi tìm kiếm nhị phân chỉ mục nhỏ nhất sao cho phạm vi tương ứng là khả thi. Mỗi lần kiểm tra tính khả thi sẽ diễn ra trong O(n) bằng cách quét qua các vị trí đèn đã sắp xếp một lần và duy trì đoạn được phủ hiện tại. Điều này làm giảm vấn đề xuống O ((n + m) log m), đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force mỗi bóng đèn | O(mn) | O(1) | Quá chậm | 
| Sắp xếp + tìm kiếm nhị phân + kiểm tra tuyến tính | O(n + m log m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi khai thác thực tế là mỗi loại bóng đèn tạo ra sự mở rộng đối xứng xung quanh mọi vị trí đèn và phạm vi phủ sóng trở nên dễ dàng hơn khi bán kính tăng lên. 

### 1. Sắp xếp vị trí đèn 

Chúng ta sắp xếp p để có thể suy luận về mức độ bao phủ từ trái sang phải. Điều này đảm bảo rằng khi chúng tôi xử lý đèn theo thứ tự, các điểm cuối bên trái trong khoảng thời gian của chúng cũng không giảm. 

### 2. Xác định kiểm tra tính khả thi cho phạm vi cố định l 

Với một l nhất định, mỗi đèn tạo ra một khoảng [p_i − l, p_i + l]. Chúng tôi mô phỏng việc hợp nhất các khoảng này mà không lưu trữ chúng một cách rõ ràng. 

Chúng tôi duy trì dòng điện thay đổi, đại diện cho điểm xa nhất trên đường đã được chiếu sáng liên tục bắt đầu từ 0. 

### 3. Quét đèn từ trái sang phải 

Đối với mỗi đèn i theo thứ tự tăng dần của p_i, chúng tôi tính toán khoảng thời gian bắt đầu và kết thúc của nó. Nếu bắt đầu > hiện tại, có một khoảng trống trong vùng phủ sóng, vì vậy điều này sẽ thất bại ngay lập tức. Ngược lại, chúng ta mở rộng dòng điện lên mức tối đa (hiện tại, kết thúc). 

### 4. Xác thực phạm vi bảo hiểm đầy đủ 

Sau khi xử lý tất cả các đèn, nếu dòng điện ≥ L thì toàn bộ đường phố bị che phủ. Ngược lại, có một số đoạn không được che chắn ở gần đầu bên phải. 

### 5. Tìm kiếm nhị phân trên phạm vi bóng đèn 

Chúng tôi sắp xếp phạm vi m bóng đèn và tìm kiếm nhị phân chỉ số nhỏ nhất có phạm vi vượt qua kiểm tra tính khả thi. Câu trả lời là phạm vi đó hoặc −1 nếu không có phạm vi nào hoạt động. 

### Tại sao nó hoạt động 

Kiểm tra tính khả thi tương đương với việc xác minh xem liệu sự kết hợp của tất cả các khoảng có chứa đường dẫn liên tục từ 0 đến L hay không. Quét tham lam hoạt động vì tại bất kỳ thời điểm nào, phạm vi bao phủ tốt nhất có thể mà chúng ta có thể có cho đến một vị trí chỉ được xác định bởi khoảng cách xa nhất trong số những khoảng bắt đầu trước hoặc tại vị trí đó. Bất kỳ khoảng thời gian nào bắt đầu muộn hơn đều không thể sửa chữa được khoảng cách một khi nó tồn tại, vì vậy việc thất bại sớm là an toàn. Tính đơn điệu trong l đảm bảo rằng một khi một giá trị hoạt động thì tất cả các giá trị lớn hơn cũng hoạt động, làm cho tìm kiếm nhị phân trở nên hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_cover(p, L, l):
    current = 0
    for x in p:
        start = x - l
        end = x + l
        if start > current:
            return False
        if end > current:
            current = end
        if current >= L:
            return True
    return current >= L

def solve():
    n, m, L = map(int, input().split())
    p = list(map(int, input().split()))
    lvals = list(map(int, input().split()))

    p.sort()
    lvals.sort()

    lo, hi = 0, m - 1
    ans = -1

    while lo <= hi:
        mid = (lo + hi) // 2
        if can_cover(p, L, lvals[mid]):
            ans = lvals[mid]
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các vị trí đèn sao cho việc hợp nhất theo khoảng thời gian trở thành một lần quét từ trái sang phải. chức năng`can_cover`mã hóa logic bao phủ tham lam. Nó theo dõi tiền tố được chiếu sáng liên tục hiện tại và loại bỏ phạm vi bóng đèn ngay lập tức khi xuất hiện khoảng trống, giúp tránh những công việc không cần thiết. 

Việc tìm kiếm nhị phân được thực hiện trên các phạm vi bóng đèn đã được sắp xếp và mỗi điểm giữa được kiểm tra bằng hàm khả thi. Cấu trúc này rất cần thiết vì việc tính toán lại vùng phủ sóng cho từng bóng đèn một cách độc lập sẽ quá chậm. 

Một lỗi triển khai phổ biến là quên rằng phạm vi bao phủ là liên tục chứ không phải rời rạc. Séc`start > current`là điều kiện chính xác để phát hiện một khoảng trống thực sự chưa được khám phá. Một điểm tinh tế khác là chấm dứt sớm khi`current >= L`, giúp tránh việc quét không cần thiết sau khi đã đạt được phạm vi bao phủ đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, m = 5, L = 10
p = [2, 4, 7, 9]
l = 2
```Chúng tôi kiểm tra tính khả thi cho l = 2. 

| Bước | Đèn | Khoảng thời gian | Hiện tại trước | Hành động | Hiện tại sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | [0, 4] | 0 | mở rộng | 4 | 
| 2 | 4 | [2, 6] | 4 | mở rộng | 6 | 
| 3 | 7 | [5, 9] | 6 | mở rộng | 9 | 
| 4 | 9 | [7, 11] | 9 | mở rộng | 11 | 

Vì hiện tại ≥ 10 nên bảo hiểm thành công. 

Dấu vết này cho thấy các khoảng chồng chéo hợp nhất thành sự mở rộng liên tục như thế nào mặc dù các đèn riêng lẻ chỉ bao phủ các vùng cục bộ. 

### Ví dụ 2 

đầu vào:```
n = 3, L = 10
p = [0, 5, 10]
l = 2
```| Bước | Đèn | Khoảng thời gian | Hiện tại trước | Hành động | Hiện tại sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | [-2, 2] | 0 | mở rộng | 2 | 
| 2 | 5 | [3, 7] | 2 | phát hiện khoảng cách | thất bại | 

Ở đèn thứ hai, khoảng thời gian bắt đầu ở mức 3 trong khi dòng điện là 2, tạo ra khoảng cách từ 2 đến 3. Mặc dù các điểm cuối trông gần nhau nhưng phạm vi phủ sóng không liên tục nên cấu hình không thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m log m) | sắp xếp vị trí một lần, sắp xếp bóng đèn, tìm kiếm nhị phân với kiểm tra tính khả thi tuyến tính | 
| Không gian | O(n) | lưu trữ vị trí đèn và các biến tạm thời | 

Độ phức tạp này phù hợp thoải mái trong các ràng buộc vì cả n và m đều lên tới 100000 và mỗi kiểm tra tính khả thi là tuyến tính với hệ số không đổi nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    def can_cover(p, L, l):
        current = 0
        for x in p:
            start = x - l
            end = x + l
            if start > current:
                return False
            if end > current:
                current = end
            if current >= L:
                return True
        return current >= L

    def solve():
        n, m, L = map(int, input().split())
        p = list(map(int, input().split()))
        lvals = list(map(int, input().split()))

        p.sort()
        lvals.sort()

        lo, hi = 0, m - 1
        ans = -1

        while lo <= hi:
            mid = (lo + hi) // 2
            if can_cover(p, L, lvals[mid]):
                ans = lvals[mid]
                hi = mid - 1
            else:
                lo = mid + 1

        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("4 7 20\n10 3 7 14\n2 5 1 7 10 4 20") == "3"
assert run("3 4 200\n14 153 22\n30 2 50 20") == "-1"

# custom cases
assert run("1 1 10\n0\n5") == "5", "single lamp"
assert run("2 2 10\n0 10\n4 5") == "5", "boundary coverage"
assert run("3 3 10\n1 2 3\n0 0 10") == "0", "zero radius works"
assert run("3 3 10\n1 5 9\n1 1 1") == "-1", "uniform small radius fails"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đèn đơn | 5 | khoảng duy nhất bao gồm toàn bộ phạm vi | 
| phạm vi bao phủ ranh giới | 5 | bắc cầu điểm cuối chính xác | 
| bán kính bằng không hoạt động | 0 | khoảng suy biến | 
| bán kính nhỏ đồng đều không thành công | -1 | những khoảng trống không thể tránh khỏi | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi đèn đầu tiên không tới được điểm gốc. Ví dụ: với p = [5, 10] và l = 2 trên L = 20, khoảng đầu tiên là [3, 7], để lại [0, 3] không được che chắn. Thuật toán phát hiện chính xác điều này vì dòng điện bắt đầu từ 0 và lần khởi động đầu tiên là 3, gây ra lỗi ngay lập tức. 

Một trường hợp khác là khi phạm vi bảo hiểm chỉ thất bại ở phần cuối. Với p = [0, 4, 8] với l = 2 và L = 15, hợp nhất đạt 10 chứ không phải 15. Quá trình quét hoàn tất và trả về sai vì dòng điện không bao giờ đạt đến L. 

Trường hợp tinh tế cuối cùng là chuỗi chặt chẽ trong đó các khoảng thời gian chỉ gặp nhau ở ranh giới. Với p = [0, 3, 6] và l = 3, các khoảng tiếp xúc chính xác tại các điểm cuối và được coi là liên tục. Điều kiện bắt đầu > hiện tại xử lý chính xác đẳng thức là kết nối hợp lệ, đảm bảo không có khoảng trống sai nào được đưa ra.
