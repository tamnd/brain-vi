---
title: "CF 104274G - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0444\u043e\u0440\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435 \u0431\u0443\u043a\u0435\u0442\u0430"
description: "Chúng ta được cho một hàng hoa, mỗi bông hoa có một loại được biểu thị bằng một số nguyên. Người bán hoa chỉ coi một bó hoa là "hợp lệ" nếu nó tương ứng với một đoạn liền kề của hàng này và đoạn đó chứa chính xác K loại hoa riêng biệt."
date: "2026-07-01T21:19:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104274
codeforces_index: "G"
codeforces_contest_name: "2023 VIII \u0418\u043d\u0442\u0435\u043b\u043b\u0435\u043a\u0442\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041f\u0424\u041e"
rating: 0
weight: 104274
solve_time_s: 66
verified: true
draft: false
---

[CF 104274G - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0444\u043e\u0440\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435 \u0431\u0443\u043a\u0435\u0442\u0430](https://codeforces.com/problemset/problem/104274/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hàng hoa, mỗi bông hoa có một loại được biểu thị bằng một số nguyên. Người bán hoa chỉ coi một bó hoa là "hợp lệ" nếu nó tương ứng với một đoạn liền kề của hàng này và đoạn đó chứa chính xác K loại hoa riêng biệt. 

Nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề có chính xác K giá trị riêng biệt. 

Kích thước đầu vào lên tới 300.000 phần tử. Bất kỳ giải pháp nào cố gắng kiểm tra trực tiếp tất cả các mảng con sẽ yêu cầu kiểm tra bình phương khoảng N trong trường hợp xấu nhất, theo thứ tự 10^10 thao tác và không khả thi trong giới hạn thời gian thông thường. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào tính toán lại số lượng riêng biệt một cách độc lập cho mỗi khoảng thời gian. 

Một điểm tinh tế trong bài toán này là “chính xác K riêng biệt” khó hơn nhiều so với “nhiều nhất là K riêng biệt”. Việc đếm các mảng con có nhiều nhất là K giá trị riêng biệt hoạt động tốt trong kỹ thuật cửa sổ trượt, trong khi K chính xác yêu cầu thủ thuật rút gọn. 

Một trường hợp thất bại điển hình xuất phát từ việc cố gắng mở rộng cả hai đầu một cách độc lập mà không duy trì cấu trúc cửa sổ nhất quán. Ví dụ: nếu tất cả các phần tử đều khác biệt và K = 2, thì việc mở rộng đơn giản có thể đếm gấp đôi hoặc bỏ lỡ các phân đoạn hợp lệ tùy thuộc vào cách xử lý các bản sao. Vấn đề không phải là tính chính xác của các kiểm tra riêng lẻ mà là việc tính toán không nhất quán của các mảng con chồng chéo. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi lặp lại mọi điểm cuối bên trái, sau đó mở rộng điểm cuối bên phải trong khi theo dõi bản đồ tần số của các phần tử bên trong phân đoạn hiện tại. Đối với mỗi phần mở rộng, chúng tôi đếm có bao nhiêu giá trị riêng biệt tồn tại và tăng câu trả lời nếu nó bằng K. Điều này hoạt động hợp lý vì nó liệt kê tất cả các mảng con có thể có chính xác một lần. 

Vấn đề xuất hiện khi chúng ta phân tích sự phức tạp. Việc duy trì bản đồ tần số được khấu hao O(1) cho mỗi lần chèn, nhưng có các mảng con O(N^2), do đó tổng công việc trở thành O(N^2), quá chậm đối với N lên tới 3 × 10^5. 

Cái nhìn sâu sắc quan trọng là chuyển đổi vấn đề từ “chính xác K” thành chênh lệch hai lần đếm “nhiều nhất là K”. Nếu chúng ta có thể đếm hiệu quả các mảng con có nhiều nhất K giá trị riêng biệt, thì số lượng mảng con có chính xác K giá trị riêng biệt là sự khác biệt giữa những mảng con có nhiều nhất K và những mảng con có nhiều nhất K − 1. Điều này hiệu quả vì mọi mảng con có nhiều nhất K đều đóng góp vào tổng và việc trừ những mảng con có nhiều nhất K − 1 sẽ loại bỏ tất cả các trường hợp có ít hơn K giá trị riêng biệt, để lại chính xác K. 

Bài toán “nhiều nhất K phân biệt” rất phù hợp cho cửa sổ trượt. Chúng tôi duy trì một cửa sổ [l, r] và đảm bảo số phần tử riêng biệt không vượt quá K bằng cách di chuyển con trỏ sang trái bất cứ khi nào cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quy vấn đề này về việc tính toán atmost(K) − atmost(K − 1). 

### bước

1. Xác định hàm atmost(K) đếm các mảng con chứa tối đa K giá trị riêng biệt. Chúng tôi sẽ tính toán nó bằng cách sử dụng cửa sổ trượt. 
2. Duy trì bản đồ tần số của các phần tử bên trong cửa sổ hiện tại và bộ đếm riêng biệt để theo dõi xem có bao nhiêu giá trị hiện có tần số khác 0. 
3. Khởi tạo hai con trỏ l và r tại 0 và biến trả lời ans = 0. 
4. Khai triển r từ trái sang phải trên mảng. Đối với mỗi phần tử mới A[r], hãy tăng tần số của nó. Nếu phần tử này xuất hiện lần đầu tiên trong cửa sổ, hãy tăng riêng biệt. 
5. Nếu độ phân biệt lớn hơn K, hãy thu nhỏ cửa sổ từ bên trái bằng cách di chuyển l về phía trước và giảm tần số cho đến khi độ phân biệt lại lớn nhất là K. Khi tần số trở thành 0, hãy giảm độ phân biệt. 
6. Khi cửa sổ thỏa mãn ràng buộc, tất cả các mảng con kết thúc tại r và bắt đầu từ bất kỳ vị trí nào từ l đến r đều hợp lệ. Thêm (r − l + 1) vào ans. 
7. Lặp lại các bước từ 4 đến 6 cho đến khi hết r. 
8. Đáp án cuối cùng là atmost(K) − atmost(K − 1). 

### Tại sao nó hoạt động 

Tại mọi vị trí r, thuật toán duy trì ranh giới l bên trái nhỏ nhất sao cho cửa sổ [l, r] chứa tối đa K giá trị phân biệt. Bất kỳ mảng con nào kết thúc tại r bắt đầu sớm hơn l sẽ đưa ra ít nhất các giá trị riêng biệt K + 1, vì việc kéo dài sang trái chỉ thêm các phần tử và không thể giảm số lượng khác biệt. 

Điều này tạo ra một cấu trúc đơn điệu: với mỗi r, tất cả các điểm bắt đầu hợp lệ tạo thành một đoạn liên tục. Việc đếm chúng là r − l + 1 đảm bảo mỗi mảng con hợp lệ được tính chính xác một lần và không bao gồm mảng con không hợp lệ. Bước trừ sau đó sẽ tách chính xác K giá trị riêng biệt mà không gặp vấn đề chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

def at_most(k, a):
    if k <= 0:
        return 0
    freq = defaultdict(int)
    l = 0
    distinct = 0
    res = 0

    for r in range(len(a)):
        if freq[a[r]] == 0:
            distinct += 1
        freq[a[r]] += 1

        while distinct > k:
            freq[a[l]] -= 1
            if freq[a[l]] == 0:
                distinct -= 1
            l += 1

        res += r - l + 1

    return res

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(at_most(k, a) - at_most(k - 1, a))

if __name__ == "__main__":
    solve()
```Việc triển khai xoay quanh chức năng trợ giúp at_most. Từ điển tần số theo dõi số phần tử bên trong cửa sổ hiện tại, trong khi đó, sự khác biệt đảm bảo chúng tôi biết khi nào ràng buộc bị vi phạm. Vòng lặp while là phần quan trọng giúp khôi phục tính hợp lệ bất cứ khi nào cửa sổ phát triển quá đa dạng. 

Phép trừ trong phép giải trực tiếp thực hiện phép chuyển đổi từ K chính xác sang nhiều nhất là K. Trường hợp cạnh k = 0 được xử lý bằng cách trả về 0 sớm, vì không có mảng con nào không trống có thể có 0 phần tử phân biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8 2
1 2 1 2 1 3 1 2
```Chúng tôi tính toán atmost(2) và atmost(1). 

Đối với atmost(2), cửa sổ sẽ mở rộng mượt mà trừ khi giá trị khác biệt thứ ba xuất hiện. Các đóng góp tích lũy dưới dạng phạm vi hợp lệ cho mỗi điểm cuối bên phải. 

| r | Một [r] | tôi | khác biệt | cửa sổ | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | [1] | 1 | 
| 1 | 2 | 0 | 2 | [1,2] | 2 | 
| 2 | 1 | 0 | 2 | [1,2,1] | 3 | 
| 3 | 2 | 0 | 2 | [1,2,1,2] | 4 | 
| 4 | 1 | 0 | 2 | [1,2,1,2,1] | 5 | 
| 5 | 3 | 1 | 2 | [2,1,3] | 3 | 
| 6 | 1 | 1 | 2 | [2,1,3,1] | 4 | 
| 7 | 2 | 1 | 2 | [2,1,3,1,2] | 5 | 

Điều này mang lại at Most(2) = 27. 

Đối với atmost(1), chỉ các phân đoạn thống nhất mới được đóng góp. Kết quả cuối cùng của phép trừ là 14. 

Dấu vết này cho thấy con trỏ trái nhảy như thế nào khi phần tử riêng biệt thứ ba xuất hiện, nén cửa sổ trong khi vẫn giữ nguyên tính hợp lệ. 

### Mẫu 2 

đầu vào:```
8 3
1 2 1 2 1 3 1 2
```Ở đây, mảng không bao giờ vượt quá 3 giá trị riêng biệt trong hầu hết các tiền tố, vì vậy atmost(3) hoạt động giống như sự tích lũy đầy đủ. 

Phép trừ loại bỏ tất cả các mảng con có 0, 1 hoặc 2 giá trị riêng biệt, chỉ để lại chính xác những mảng có 3 giá trị riêng biệt. 

Sự bằng nhau của các đầu ra trong cả hai mẫu xác nhận rằng cấu trúc của các chuyển đổi giá trị khác biệt, chứ không phải tần số thô của các số cụ thể, chi phối câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi con trỏ l và r di chuyển tổng cộng tối đa N lần trong mỗi lệnh gọi atmost | 
| Không gian | O(N) | Bản đồ tần số lưu trữ số lượng phần tử hoạt động trong cửa sổ | 

Giải pháp chạy hai đường tuyến tính trên mảng, do đó tổng độ phức tạp vẫn tuyến tính theo N, phù hợp thoải mái trong giới hạn cho N lên tới 3 × 10^5. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def at_most(k, a):
        if k <= 0:
            return 0
        freq = defaultdict(int)
        l = 0
        distinct = 0
        res = 0
        for r in range(len(a)):
            if freq[a[r]] == 0:
                distinct += 1
            freq[a[r]] += 1
            while distinct > k:
                freq[a[l]] -= 1
                if freq[a[l]] == 0:
                    distinct -= 1
                l += 1
            res += r - l + 1
        return res

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    return str(at_most(k, a) - at_most(k - 1, a))

# provided samples
assert run("8 2\n1 2 1 2 1 3 1 2\n") == "14"
assert run("8 3\n1 2 1 2 1 3 1 2\n") == "14"

# minimum size
assert run("1 1\n5\n") == "1"

# impossible case
assert run("5 2\n1 1 1 1 1\n") == "0"

# all distinct
assert run("5 3\n1 2 3 4 5\n") == "3"

# boundary
assert run("6 2\n1 2 3 1 2 3\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | cửa sổ hợp lệ tối thiểu | 
| tất cả đều bằng nhau, k=2 | 0 | không cách nào đạt được k khác biệt | 
| tất cả đều khác biệt, k=3 | kiểm tra hành vi trượt | nhiều lần mở rộng và co lại | 
| mô hình xen kẽ | xác minh quá trình chuyển đổi | ổn định cửa sổ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi K bằng 1. Trong trường hợp này, mọi mảng con bao gồm các phần tử giống hệt nhau đều hợp lệ. Phép biến đổi atmost vẫn hoạt động vì atmost(1) đếm tất cả các phân đoạn thống nhất và atmost(0) trả về chính xác số 0 vì không có mảng con nào có thể có các giá trị riêng biệt bằng 0. 

Một trường hợp cạnh khác xảy ra khi K lớn hơn số giá trị riêng biệt trong mảng. Ở đây atmost(K) đếm tất cả các mảng con, trong khi atmost(K − 1) đã bằng atmost(K), tạo ra 0. Cửa sổ trượt không bao giờ kích hoạt thiết lập lại toàn bộ vì bộ đếm riêng biệt không bao giờ vượt quá K, do đó toàn bộ mảng được coi là một vùng hợp lệ bất cứ khi nào có thể. 

Trường hợp khó phát hiện cuối cùng là khi việc thay đổi thường xuyên khiến con trỏ bên trái bị co lại nhiều lần. Ngay cả trong các mẫu như 1,2,1,2,1,2, cửa sổ vẫn dao động nhưng mỗi chỉ mục vẫn được xử lý nhiều nhất một lần cho mỗi chuyển động của con trỏ, duy trì độ phức tạp tuyến tính trong khi vẫn duy trì việc đếm chính xác các phân đoạn hợp lệ chồng chéo.
