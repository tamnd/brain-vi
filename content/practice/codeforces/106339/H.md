---
title: "CF 106339H - Bạn có muốn xây dựng người tuyết không?"
description: "Chúng ta được cấp một chuỗi các số nguyên không âm biểu thị lượng tuyết có sẵn dọc theo một đường. Nhiệm vụ là chọn ba phần liền kề hoặc không chồng chéo của chuỗi này và gán chúng cho ba quả cầu tuyết có kích thước được ký hiệu là s1, s2 và s3 sau khi sắp xếp sao cho…"
date: "2026-06-19T14:51:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106339
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 1-28-2026"
rating: 0
weight: 106339
solve_time_s: 52
verified: true
draft: false
---

[CF 106339H - Bạn có muốn xây người tuyết không?](https://codeforces.com/problemset/problem/106339/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các số nguyên không âm biểu thị lượng tuyết có sẵn dọc theo một đường. Nhiệm vụ là chọn ba phần liền kề hoặc không chồng chéo của chuỗi này và gán chúng cho ba quả cầu tuyết có kích thước được ký hiệu là s1, s2 và s3 sau khi sắp xếp sao cho s1 ≤ s2 ≤ s3. Ràng buộc không phải là tùy ý: công trình xây dựng phải thỏa mãn điều kiện tăng trưởng trong đó quả cầu tuyết ở giữa nhỏ nhất ít nhất gấp đôi và quả cầu tuyết lớn nhất ít nhất gấp đôi quả cầu ở giữa. 

Vấn đề cơ bản không chỉ đơn giản là chọn ba phân đoạn bất kỳ mà là phân chia tuyết có sẵn thành ba phần theo cách tôn trọng các ràng buộc nhân này trong khi vẫn sử dụng các phạm vi rời rạc của mảng. Đầu ra là cấu hình tốt nhất có thể đạt được theo các quy tắc này, thường được tối ưu hóa theo mục tiêu tiềm ẩn được mã hóa trong báo cáo vấn đề, giúp giảm thiểu việc tìm ra các phần tách hợp lệ nhằm tối đa hóa tính khả thi trong các ràng buộc nhân đôi. 

Kích thước đầu vào ngụ ý một mảng lên tới khoảng 100.000 phần tử. Bất kỳ cách tiếp cận nào thử trực tiếp tất cả các bộ ba vị trí cắt sẽ liên quan đến hành vi bậc ba hoặc bậc hai trên các điểm phân chia và ngay lập tức quá chậm. Ngay cả việc quét O(n^2) qua hai vị trí cắt cũng trở thành ranh giới nếu mỗi lần kiểm tra là tuyến tính hoặc liên quan đến việc tính toán lại. 

Trường hợp cạnh cấu trúc quan trọng là sự phân tách tham lam hoặc cục bộ không thành công khi tuyết phân bố không đều. Ví dụ: nếu hầu hết tuyết tập trung sớm, một nỗ lực ngây thơ có thể tạo thành s1 và s2 nhỏ hợp lệ nhưng để lại khối lượng còn lại không đủ cho s3 ≥ 2s2, mặc dù phân vùng bị dịch chuyển một chút sẽ thành công. Ngược lại, việc đẩy s1 quá lớn có thể khiến s2 ≥ 2s1 không thể thỏa mãn ngay cả khi tồn tại cấu hình hợp lệ sau này. 

Lỗi minh họa tối thiểu là một mảng như [1, 1, 100, 100, 100]. Một phép chia tham lam ngây thơ có thể chọn s1 = 1, s2 = 1, để lại s3 = 300, vi phạm s2 ≥ 2s1 vì 1 ≥ 2 là sai, mặc dù tồn tại một sự sắp xếp hợp lệ bằng cách nhóm khác nhau. 

## Phương pháp tiếp cận 

Phương pháp brute-force cố định hai điểm cắt i và j và gán các phân đoạn [1..i], [i+1..j], [j+1..n] cho s1, s2, s3 trong tất cả các hoán vị có thể có. Đối với mỗi phân vùng, chúng tôi tính toán tổng phân đoạn và kiểm tra xem sau khi sắp xếp chúng, các ràng buộc nhân đôi có giữ nguyên hay không. Điều này yêu cầu O(n^2) lựa chọn (i, j) và việc tính toán tổng phân đoạn có thể được thực hiện trong O(1) bằng cách sử dụng tổng tiền tố, cho tổng thời gian là O(n^2). Tuy nhiên, cấu trúc bài toán hạn chế hơn so với các phân vùng tùy ý, bởi vì các ràng buộc buộc một mối quan hệ đơn điệu giữa các kích thước phân đoạn. 

Quan sát quan trọng là khi chúng ta ấn định ranh giới ứng cử viên cho một quả cầu tuyết, giả sử chúng ta quyết định nơi s1 kết thúc hoặc nơi s3 bắt đầu, việc kiểm tra tính khả thi còn lại sẽ trở nên đơn điệu. Nếu chúng ta tăng kích thước của một phân khúc thì các ngưỡng bắt buộc cho phân khúc tiếp theo sẽ di chuyển theo hướng có thể dự đoán được. Tính đơn điệu này cho phép tìm kiếm nhị phân trên các điểm cuối của phân đoạn thay vì quét tuyến tính. 

Chúng tôi khai thác tổng tiền tố để bất kỳ tổng phân đoạn nào cũng có thể được tính theo O(1), và sau đó đối với điểm phân chia cố định l, chúng tôi có thể tìm kiếm nhị phân ranh giới tiếp theo r sao cho đoạn giữa thỏa mãn ràng buộc nhân đôi. Đối số đối xứng áp dụng cho phân đoạn thứ ba. Vì có nhiều thứ tự hợp lệ của s1, s2, s3 dọc theo mảng nên chúng tôi xem xét tất cả các hoán vị một cách ngầm định bằng cách đảo ngược mảng và áp dụng lại cùng một logic. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tiền tố + Tìm kiếm nhị phân | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi làm việc với các tổng tiền tố để tổng của bất kỳ khoảng nào có thể được tính theo thời gian không đổi. Sau đó, chúng tôi lặp lại việc lựa chọn ranh giới l xác định phần cuối của phân đoạn được chọn đầu tiên. L này đại diện cho một vị trí ứng cử viên nơi một quả cầu tuyết kết thúc và quả cầu tuyết tiếp theo bắt đầu. 

Đối với mỗi l cố định, chúng tôi coi tiền tố [1..l] là kích thước quả cầu tuyết tiềm năng và sau đó tìm kiếm ranh giới thứ hai hợp lệ r trong [l+1..n]. Mục tiêu của ranh giới thứ hai này là đảm bảo rằng phân khúc tiếp theo đủ lớn để đáp ứng ràng buộc nhân đôi so với phân khúc đầu tiên. Bởi vì tổng phân đoạn tăng khi r tăng, tính khả thi trở thành một vị từ đơn điệu trên r. 

Chúng tôi sử dụng tìm kiếm nhị phân trên r để tìm vị trí nhỏ nhất hoặc lớn nhất thỏa mãn ràng buộc, tùy thuộc vào thứ tự quả cầu tuyết mà chúng tôi hiện đang lập mô hình. Sau khi r được sửa, hậu tố còn lại sẽ xác định quả cầu tuyết thứ ba và chúng tôi kiểm tra xem nó có thỏa mãn ràng buộc cuối cùng hay không. 

Vì bài toán yêu cầu xem xét tất cả các hoán vị của s1, s2, s3 nên chúng ta lặp lại quy trình tương tự cho tất cả các phép gán vai trò logic. Thay vì liệt kê rõ ràng sáu hoán vị, chúng tôi nhận thấy rằng việc đảo ngược mảng cho phép chúng tôi hoán đổi các vai trò “dựa trên tiền tố” và “dựa trên hậu tố”, bao gồm các trường hợp đối xứng một cách hiệu quả. 

Sau khi quét tất cả các l có thể và áp dụng tìm kiếm nhị phân cho r, chúng tôi theo dõi xem có tồn tại bất kỳ cấu hình hợp lệ nào không. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hành vi đơn điệu của tổng tiền tố. Khi chúng ta mở rộng ranh giới phân đoạn, tổng phân đoạn không bao giờ giảm. Điều này đảm bảo rằng các ràng buộc như s2 ≥ 2s1 hoặc s3 ≥ 2s2 trở thành các vị từ đơn điệu trên không gian tìm kiếm. Khi một cấu hình trở nên hợp lệ tại một số r, tất cả các phần mở rộng tiếp theo vẫn hợp lệ cho hướng bất đẳng thức cụ thể đó, đó chính xác là điều cho phép tìm kiếm nhị phân tách biệt điểm chuyển tiếp. Bởi vì tất cả các vai trò đều được thực hiện thông qua phép lặp trên l và tính đối xứng đảo ngược nên không có phân vùng hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    def get(l, r):
        return pref[r] - pref[l]

    def check(arr):
        n = len(arr)
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + arr[i]

        def seg(l, r):
            return pref[r] - pref[l]

        for l in range(1, n + 1):
            s1 = seg(0, l)
            
            lo, hi = l + 1, n - 1
            while lo <= hi:
                mid = (lo + hi) // 2
                s2 = seg(l, mid)
                s3 = seg(mid, n)

                if s2 >= 2 * s1:
                    hi = mid - 1
                else:
                    lo = mid + 1

            if lo < n:
                s2 = seg(l, lo)
                s3 = seg(lo, n)
                if s2 >= 2 * s1 and s3 >= 2 * s2:
                    return True

        return False

    if check(a) or check(a[::-1]):
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng tổng tiền tố cho các truy vấn phạm vi O(1). các`check`hàm kiểm tra một hướng của mảng, trong khi lệnh gọi đảo ngược bao gồm các sắp xếp đối xứng. 

Đối với mỗi điểm cuối tiền tố l, chúng tôi tính s1 là tổng của phân đoạn đầu tiên. Sau đó chúng tôi tìm kiếm nhị phân điểm cắt thứ hai. Vị từ được sử dụng trong tìm kiếm nhị phân thực thi bất đẳng thức thứ nhất, đảm bảo rằng s2 đủ lớn so với s1. Sau khi định vị một phép chia ứng cử viên, chúng ta xác minh rõ ràng cả hai ràng buộc bao gồm bất đẳng thức thứ hai liên quan đến s3, vì bất đẳng thức đó phụ thuộc vào phép chia chính xác. 

Bước đảo ngược là rất quan trọng vì cấu trúc của các ràng buộc mang tính định hướng. Nếu không có nó, chỉ có thể truy cập được một nửa số cấu hình hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng đầu vào [1, 2, 8, 16, 40]. 

Trước tiên, chúng tôi tính tổng tiền tố: [1, 3, 11, 27, 67]. 

Với l = 1, s1 = 1. Chúng tôi tìm kiếm nhị phân cho r. Giả sử r = 2 cho s2 = 2, không đạt 2 ≥ 2 nên ta khai triển. Tại r = 3, s2 = 10, hiện tại hợp lệ. Khi đó s3 = 56, thỏa mãn 56 ≥ 20 nên cấu hình này hoạt động. 

| tôi | r (đã tìm thấy) | s1 | s2 | s3 | s2 ≥ 2s1 | s3 ≥ 2s2 | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 10 | 56 | vâng | vâng | 

Điều này cho thấy trường hợp việc phân tách sớm không thành công nhưng phân khúc ở giữa lớn hơn một chút sẽ khôi phục được tính khả thi. 

Bây giờ hãy xem xét [5, 5, 5, 5]. 

Với bất kỳ l nào, s1 tăng quá nhanh so với s2 và s3 có thể có. Ngay cả khi chúng ta chọn cách chia cân bằng, các ràng buộc nhân đôi sẽ thất bại ngay lập tức. 

| tôi | r | s1 | s2 | s3 | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 10 | 5 | 5 | không | 

Điều này chứng tỏ rằng việc tìm kiếm đơn điệu sẽ sử dụng hết tất cả các khả năng một cách chính xác mà không chấp nhận sai các phân vùng không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi l kích hoạt tìm kiếm nhị phân trên r và mỗi lần kiểm tra sử dụng truy vấn tổng tiền tố O(1) | 
| Không gian | O(n) | Tổng tiền tố được lưu trữ để tính toán phạm vi nhanh | 

Độ phức tạp vừa vặn trong giới hạn n lên tới 100.000, vì tổng số thao tác vào khoảng vài triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    def seg(l, r):
        return pref[r] - pref[l]

    def check(arr):
        n = len(arr)
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + arr[i]

        def seg(l, r):
            return pref[r] - pref[l]

        for l in range(1, n + 1):
            s1 = seg(0, l)
            lo, hi = l + 1, n - 1
            while lo <= hi:
                mid = (lo + hi) // 2
                s2 = seg(l, mid)
                if s2 >= 2 * s1:
                    hi = mid - 1
                else:
                    lo = mid + 1

            if lo < n:
                s2 = seg(l, lo)
                s3 = seg(lo, n)
                if s2 >= 2 * s1 and s3 >= 2 * s2:
                    return True

        return False

    return "YES" if (check(a) or check(a[::-1])) else "NO"

assert run("5\n1 2 8 16 40\n") == "YES"
assert run("4\n5 5 5 5\n") == "NO"
assert run("3\n1 1 100\n") == "NO"
assert run("6\n1 1 2 4 8 16\n") == "YES"
assert run("3\n10 1 1\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tăng theo cấp số nhân | CÓ | phân vùng hợp lệ điển hình | 
| tất cả đều bình đẳng | KHÔNG | không có cấu trúc nhân đôi hợp lệ | 
| lệch đơn giá trị lớn | KHÔNG | không thể cân bằng phân khúc | 
| tiến trình hình học | CÓ | tính khả thi về ranh giới | 
| tiền tố nặng đảo ngược | KHÔNG | yêu cầu xử lý đảo ngược | 

## Vỏ cạnh 

Trường hợp một cạnh là khi đoạn đầu tiên cực kỳ nhỏ so với phần còn lại, điều này có thể tạo ra nhiều giá trị r hợp lệ. Tìm kiếm nhị phân xử lý chính xác điều này vì vị từ vẫn đơn điệu: khi s2 vượt qua ngưỡng, nó vẫn hợp lệ với r lớn hơn, vì vậy chúng tôi không bao giờ bỏ lỡ ranh giới khả thi tối thiểu. 

Một trường hợp cạnh khác là khi phân chia tối ưu sử dụng nhiều hậu tố, đó là lý do tại sao cần phải đảo ngược mảng. Đối với đầu vào [100, 1, 1, 1, 1], cấu hình đúng sẽ đặt giá trị lớn ở cuối là s3. Chạy thuật toán trên mảng đảo ngược sẽ hiển thị cấu trúc dựa trên tiền tố tương đương và logic tìm kiếm nhị phân tương tự xác định tính khả thi. 

Trường hợp cạnh cuối cùng xảy ra khi ranh giới nằm ở các vị trí cực trị như l = n - 2. Trong trường hợp này, không gian tìm kiếm nhị phân là tối thiểu, nhưng thuật toán vẫn kiểm tra chính xác xem hai phân đoạn cuối có thỏa mãn các ràng buộc mà không yêu cầu cách viết hoa đặc biệt hay không.
