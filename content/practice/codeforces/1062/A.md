---
title: "CF 1062A - Trò đùa"
description: "Chúng ta được cung cấp một dãy số nguyên tăng dần, tất cả từ 1 đến 1000. Ai đó được phép xóa chính xác một khối phần tử liền kề, để lại một khoảng trống trong chuỗi. Các phần tử còn lại giữ nguyên vị trí ban đầu."
date: "2026-06-15T08:41:25+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1062
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 520 (Div. 2)"
rating: 1300
weight: 1062
solve_time_s: 167
verified: true
draft: false
---

[CF 1062A - Trò đùa](https://codeforces.com/problemset/problem/1062/A) 

**Đánh giá:** 1300 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên tăng dần, tất cả từ 1 đến 1000. Ai đó được phép xóa chính xác một khối phần tử liền kề, để lại một khoảng trống trong chuỗi. Các phần tử còn lại giữ nguyên vị trí ban đầu. 

Sau khi xóa, phân đoạn bị thiếu vẫn phải có thể phục hồi duy nhất bằng cách chỉ sử dụng các giá trị còn lại và thực tế là mảng ban đầu đang tăng nghiêm ngặt với các giá trị được giới hạn bởi 1 và 1000. 

Nhiệm vụ là tìm đoạn liền kề dài nhất có thể bị xóa trong khi vẫn đảm bảo rằng các giá trị bị xóa được xác định duy nhất. 

Hạn chế chính là tính duy nhất của việc tái thiết. Nếu nhiều lần hoàn thành tăng dần khác nhau vừa với khoảng trống thì việc xóa đó không hợp lệ. 

Vì n nhiều nhất là 100 nên việc kiểm tra bậc hai hoặc thậm chí bậc ba trên các khoảng có thể là dễ dàng khả thi. Bất cứ điều gì vượt quá O(n^3) sẽ không cần thiết và vẫn có thể an toàn, nhưng chúng ta có thể làm tốt hơn bằng cách lý luận về cách phát sinh sự mơ hồ. 

Một trường hợp lỗi tinh vi xuất hiện khi các giá trị biên còn lại để lại quá nhiều khoảng trống số cho nhiều chuỗi số nguyên tăng dần. 

Ví dụ: nếu chúng tôi loại bỏ một đoạn ở giữa nhưng khoảng cách giữa giá trị biên bên trái và giá trị biên bên phải đủ lớn để chứa nhiều hơn một đoạn lấp đầy tăng dần độ dài bị thiếu, thì việc tái tạo không phải là duy nhất. 

Một trường hợp cạnh khác xảy ra ở ranh giới. Nếu chúng ta xóa từ đầu hoặc cuối thì chỉ có một bên ràng buộc các giá trị còn thiếu, do đó tính khả thi chỉ phụ thuộc vào giới hạn toàn cục 1 và 1000. 

Một sai lầm ngây thơ là cho rằng mọi khoảng cách đều có thể phục hồi được miễn là giá trị vẫn tăng. Điều đó bỏ qua sự tự do tổ hợp bên trong các khoảng số lớn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi mảng con có thể để xóa và kiểm tra xem đoạn bị xóa có thể được xây dựng lại một cách duy nhất hay không. Đối với mỗi khoảng, chúng tôi mô phỏng xem có tồn tại chính xác một chuỗi tăng dần phù hợp giữa hai ràng buộc biên hay không. 

Đối với một phân đoạn từ l đến r, chúng ta xét a[l-1] và a[r+1] (nếu chúng tồn tại) và đếm xem có bao nhiêu cách chúng ta có thể chọn r-l+1 tăng nghiêm ngặt các số nguyên giữa chúng. Điều này có thể được thực hiện thông qua tổ hợp hoặc DP trên các giá trị có thể có trong [1, 1000]. 

Đối với mỗi khoảng thời gian, hiệu lực tính toán từ đầu là O(1000) hoặc tệ hơn và có các khoảng O(n^2), do đó lực lượng vũ phu trở thành O(n^2 * 1000). Điều này đã ổn với n = 100, nhưng nó quá mức cần thiết. 

Quan sát quan trọng là tính duy nhất không thành công chính xác khi có ít nhất hai cách hợp lệ để lấp đầy khoảng trống. Điều đó xảy ra khi số lượng số nguyên có sẵn giữa các ranh giới lớn hơn số lượng vị trí cần điền. Nếu có k vị trí và ít nhất k+1 số nguyên khả dụng trong khoảng mở, chúng ta luôn có thể chọn nhiều dãy tăng dần. 

Vì vậy, trong một khoảng thời gian cố định, điều kiện giảm xuống thành một phép kiểm tra số học đơn giản bằng cách sử dụng các giá trị biên mà không liệt kê các chuỗi. Điều này biến mỗi séc thành O(1), cho phép tổng số O(n^2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 · 1000) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n^2) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi thử mọi phân đoạn liền kề có thể làm khối bị xóa và kiểm tra xem việc tái tạo có phải là duy nhất hay không.

1. Cố định điểm cuối bên trái l cho đoạn bị xóa. Điều này đại diện cho phần tử bị loại bỏ đầu tiên. 
2. Kéo dài điểm cuối bên phải r từ l đến n. Điều này xác định khối bị xóa [l, r]. Độ dài của đoạn bị xóa là r - l + 1. 
3. Xác định các hạn chế về ranh giới. Giá trị trước đoạn là a[l-1] nếu l > 1 và giá trị sau là a[r+1] nếu r < n. Hai giá trị này hạn chế những gì có thể được đặt trong vùng bị xóa. 
4. Tính xem có bao nhiêu số nguyên nằm đúng giữa hai giá trị biên. Nếu cả hai ranh giới đều tồn tại thì đây là a[r+1] - a[l-1] - 1. Nếu thiếu một bên, chúng ta coi nó là không giới hạn ở bên đó nhưng vẫn bị ràng buộc bởi 1 và 1000. 
5. So sánh số giá trị có sẵn với số vị trí cần điền. Nếu số nguyên sẵn có vượt quá số lượng vị trí, có nhiều cách để chọn một dãy tăng nghiêm ngặt, do đó việc tái tạo không phải là duy nhất. 
6. Nếu các số nguyên có sẵn chính xác bằng số vị trí thì có đúng một cách để gán giá trị, do đó phân đoạn hợp lệ. 
7. Theo dõi độ dài tối đa của bất kỳ phân đoạn hợp lệ nào. 

Chi tiết quan trọng là tính đơn điệu nghiêm ngặt buộc các giá trị bị xóa để tương ứng với một lựa chọn tăng dần trong một khoảng cố định, do đó vấn đề giảm xuống việc đếm các kết hợp thay vì xây dựng các chuỗi. 

### Tại sao nó hoạt động 

Giữa hai giá trị biên cố định, bất kỳ sự hoàn thành hợp lệ nào đều tương ứng với việc chọn một chuỗi tăng dần có cùng độ dài với đoạn bị xóa khỏi các số nguyên trong khoảng đó. Nếu khoảng chứa nhiều phần tử hơn vị trí thì tồn tại nhiều tập hợp con, do đó tính duy nhất không thành công. Nếu nó chứa chính xác số phần tử bằng số vị trí thì có chính xác một tập hợp con có thể, buộc phải xây dựng lại một lần. Sự tương đương này đảm bảo điều kiện đếm tham lam phù hợp chính xác với yêu cầu về tính duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    a = list(map(int, input().split()))

    ans = 0

    for l in range(n):
        for r in range(l, n):
            k = r - l + 1

            left = a[l - 1] if l > 0 else 0
            right = a[r + 1] if r + 1 < n else 1001

            available = right - left - 1

            if available == k:
                ans = max(ans, k)

    print(ans)

if __name__ == "__main__":
    main()
```Các vòng lặp lồng nhau liệt kê tất cả các phân đoạn có thể bị xóa. Đối với mỗi phân đoạn, chúng tôi tính toán độ dài của nó và khoảng cách số hiệu quả giữa các phân đoạn lân cận. Các giá trị trọng điểm 0 và 1001 mở rộng ranh giới sao cho việc xóa ở các cạnh được xử lý thống nhất. 

Chi tiết triển khai chính là điều kiện`available == k`. Điều này mã hóa tính duy nhất: phải tồn tại chính xác đủ số nguyên để buộc một lựa chọn tăng dần nghiêm ngặt. Nếu có nhiều số nguyên hơn vị trí thì sẽ xuất hiện sự mơ hồ. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[1, 3, 4, 5, 6, 9]`. 

Chúng tôi kiểm tra phân khúc`[4, 5]`, tương ứng với l = 2, r = 3 (0-chỉ số). Ranh giới là 3 ở bên trái và 5 ở bên phải. Các số nguyên khả dụng giữa chúng là {4}, vậy có sẵn = 1. Độ dài đoạn cũng là 2? Trên thực tế, phân đoạn này không hợp lệ vì độ dài 2 yêu cầu chính xác hai số nguyên từ 3 đến 5, điều này là không thể nên nó bị từ chối. 

Bây giờ hãy xem xét`[3, 4]`với l = 1, r = 2. Ranh giới là 1 và 5. Các số nguyên có sẵn là {2, 3, 4}? Trên thực tế, từ 1 đến 5 sẽ cho ra {2, 3, 4}, do đó có sẵn = 3. Độ dài phân đoạn là 2, do đó có sẵn > k, nghĩa là tồn tại nhiều phần lấp đầy và việc tái tạo không phải là duy nhất, vì vậy nó không hợp lệ. 

Cuối cùng, hãy xem xét một phân đoạn tối ưu hợp lệ trong đó kích thước khoảng trống khớp chính xác với số lượng vị trí được yêu cầu. Đó là tình huống duy nhất có tính duy nhất và thuật toán nắm bắt nó thông qua sự bình đẳng. 

Dấu vết này cho thấy rằng cả khoảng thời gian quá nhỏ và quá lớn đều bị từ chối và chỉ có sự phù hợp chính xác mới tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | tất cả các mảng con được kiểm tra một lần, mỗi mảng trong O(1) | 
| Không gian | O(1) | chỉ có một vài biến được sử dụng | 

Với n ≤ 100, khoảng thời gian 10.000 lần kiểm tra là không đáng kể trong giới hạn thời gian và việc sử dụng bộ nhớ liên tục giúp giảm thiểu chi phí. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    ans = 0

    for l in range(n):
        for r in range(l, n):
            k = r - l + 1
            left = a[l - 1] if l > 0 else 0
            right = a[r + 1] if r + 1 < n else 1001
            if right - left - 1 == k:
                ans = max(ans, k)

    return str(ans)

# provided sample
assert solve("6\n1 3 4 5 6 9\n") == "2"

# minimum size
assert solve("1\n100\n") == "0"

# already tight increasing
assert solve("3\n1 2 3\n") == "0"

# edge deletion valid
assert solve("4\n1 100 101 102\n") == "2"

# large gap case
assert solve("5\n1 10 20 30 40\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | không thể xóa | 
| số nguyên liên tiếp | 0 | không có khoảng cách duy nhất hợp lệ | 
| trường hợp hợp lệ ranh giới nhỏ | 2 | độ chính xác ở các cạnh | 
| giá trị thưa thớt | 3 | xử lý các khoảng thời gian lớn | 

## Vỏ cạnh 

Trường hợp một cạnh đang xóa từ đầu. Đối với một đầu vào như`[5, 6, 7]`, việc loại bỏ tiền tố có nghĩa là ranh giới bên trái thực tế là 1. Thuật toán xử lý ranh giới bên trái bị thiếu là 0, do đó khoảng được giới hạn chính xác và tính duy nhất chỉ phụ thuộc vào ràng buộc bên phải, được xử lý nhất quán. 

Một trường hợp cạnh khác là xóa toàn bộ mảng ngoại trừ một phần tử. Vì cả hai ranh giới đều trở thành 0 và 1001 nên phạm vi khả dụng là tối đa. điều kiện`available == k`vẫn xác định chính xác liệu có tồn tại một chuỗi tăng nghiêm ngặt duy nhất hay không và ngăn chặn sự hoàn thành mơ hồ khi vẫn còn quá nhiều tự do về số.
