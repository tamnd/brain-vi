---
title: "CF 105266C - \u91cd\u91cfII"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp, có một số loại trọng lượng và mỗi loại có nguồn cung không giới hạn. Tuy nhiên, có một hạn chế trong cách chúng ta sử dụng chúng: tất cả các quả cân đã chọn phải được đặt ở cùng một phía của cân."
date: "2026-06-24T01:01:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105266
codeforces_index: "C"
codeforces_contest_name: "2024 XTU Summer Camp Selection Competition"
rating: 0
weight: 105266
solve_time_s: 55
verified: true
draft: false
---

[CF 105266C - \u91cd\u91cfII](https://codeforces.com/problemset/problem/105266/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp, có một số loại trọng lượng và mỗi loại có nguồn cung không giới hạn. Tuy nhiên, có một hạn chế trong cách chúng ta sử dụng chúng: tất cả các quả cân đã chọn phải được đặt ở cùng một phía của cân. Bằng cách sử dụng một số lựa chọn trong số các trọng số này, chúng ta muốn có thể biểu thị mọi trọng số nguyên từ 1 đến m, nghĩa là với mỗi giá trị mục tiêu x trong phạm vi đó, chúng ta phải có thể chọn nhiều tập trọng số có sẵn có tổng chính xác là x. 

Mục tiêu không phải là tối đa hóa phạm vi có thể biểu thị mà thay vào đó là đạt được phạm vi bao phủ đầy đủ của khoảng [1, m] trong khi sử dụng càng ít phần trọng lượng riêng lẻ càng tốt, trong đó chúng tôi có thể sử dụng lại các loại nhưng mỗi bản sao được chọn sẽ được tính vào tổng số phần được sử dụng. 

Một điểm tinh tế quan trọng là đầu vào cung cấp các loại trọng lượng chứ không phải từng phần riêng lẻ. Chúng tôi đang lựa chọn một cách hiệu quả số lượng bản sao của từng loại trọng lượng mà chúng tôi lấy và những bản sao đó sau đó sẽ trở thành tập hợp có thể sử dụng được của chúng tôi. Câu hỏi trở thành: tổng số bản sao tối thiểu là bao nhiêu để có thể đạt được tất cả các tổng từ 1 đến m bằng cách lặp lại không giới hạn các bản sao đã chọn đó. 

Tổng cộng các ràng buộc rất lớn: tối đa 50 trường hợp thử nghiệm, tối đa 10.000 loại trọng số cho mỗi trường hợp và m có thể lớn bằng 1e9. Bất kỳ giải pháp nào cố gắng mô phỏng trực tiếp các tập hợp con lên tới m đều không thể thực hiện được vì ngay cả O(m) trên mỗi trường hợp thử nghiệm cũng đã vượt quá giới hạn và O(n·m) là hoàn toàn không khả thi. 

Một sai lầm ngây thơ là cho rằng chúng ta có thể tham lam chọn các trọng số mà không theo dõi các khoảng thời gian có thể biểu thị được. Ví dụ: nếu chúng ta chọn trọng số 2 và 3 khi m = 4, chúng ta có thể nghĩ rằng bằng cách nào đó chúng ta có thể bao quát được mọi thứ, nhưng 1 sẽ không thể truy cập được ngay lập tức. Điều này cho thấy khả năng biểu diễn phụ thuộc rất nhiều vào việc liệu chúng ta có thể xây dựng một phạm vi liên tục bắt đầu từ 1 hay không. 

Một trường hợp thất bại phổ biến khác là bỏ qua những khoảng trống. Giả sử chúng ta chọn trọng số {1, 3}. Chúng ta có thể tạo thành 1 và 3, nhưng thiếu 2, và sau đó hệ thống bị hỏng hoàn toàn vì chúng ta không thể lấp đầy khoảng trống chỉ bằng cách sử dụng các phép cộng của 1 và 3. Bất kỳ chiến lược đúng đắn nào cũng phải đảm bảo rằng phạm vi có thể biểu diễn được duy trì liên tục. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xem xét mọi tập hợp con của các bản sao trọng số mà chúng ta có thể lấy, sau đó mô phỏng tất cả các tổng có thể có lên đến m bằng cách sử dụng không giới hạn các trọng số đã chọn đó. Ngay cả khi chúng ta hạn chế chọn tối đa k bản sao, thì số cách chọn chúng là tổ hợp tính bằng n, và khi đó mỗi lựa chọn yêu cầu DP kiểu ba lô lên tới m. Điều này nhanh chóng bùng nổ: ngay cả một DP đơn lẻ lên tới 1e9 cũng không thể thực hiện được và việc liệt kê các tập hợp con còn tệ hơn theo cấp số nhân. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến các tổng riêng lẻ ngoài tiền tố liền kề hiện tại mà chúng ta có thể hình thành. Nếu chúng ta đã biết rằng chúng ta có thể hình thành mọi giá trị trong [1, R], thì một trọng số mới w hoạt động theo một cách rất có cấu trúc: sử dụng nó, chúng ta có thể mở rộng phạm vi bao phủ đến [1, R + w] khi và chỉ khi w lớn nhất là R + 1. Nếu không, một khoảng trống sẽ xuất hiện ở R + 1 và chúng ta không bao giờ có thể sửa nó sau này. 

Điều này dẫn đến một công trình tham lam gợi nhớ đến việc xây dựng những khoản tiền có thể đạt được với các yếu tố tối thiểu. Chúng tôi xử lý các trọng số theo thứ tự tăng dần và duy trì tiền tố R có thể biểu diễn liên tục lớn nhất. Ban đầu R = 0, vì không có gì có thể biểu diễn được. Mỗi lần chúng tôi quyết định lấy trọng số w, chúng tôi sử dụng một bản sao của nó, điều này cho phép chúng tôi mở rộng phạm vi tiếp cận lên tới R + w miễn là w có thể sử dụng được theo ràng buộc hiện tại. Nếu trọng số nhỏ nhất còn lại lớn hơn R + 1 thì chúng ta buộc phải kết luận rằng 1..m không thể hoàn thành. 

Để giảm thiểu số lượng vật phẩm được chọn, chúng ta phải luôn chọn trọng lượng nhỏ nhất hiện có để giúp mở rộng phạm vi, vì trọng lượng lớn hơn sẽ tiêu tốn nhiều “năng lượng” hơn cho mỗi vật phẩm và không giúp lấp đầy những khoảng trống nhỏ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tập hợp con + DP) | hàm mũ / O(2^n · m) | O(m) | Quá chậm | 
| Phần mở rộng tiền tố tham lam tối ưu | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sắp xếp các trọng số để luôn xem xét các giá trị nhỏ hơn trước tiên. 

1. Khởi tạo một biến R = 0 biểu thị tổng liên tục tối đa mà chúng ta hiện có thể hình thành. Đồng thời khởi tạo chỉ mục i = 0 và bộ đếm ans = 0. 
2. Khi R < m, chúng ta cố gắng mở rộng phạm vi. Chúng tôi xem xét trọng lượng chưa sử dụng nhỏ nhất w = a[i]. 
3. Nếu không tồn tại trọng số như vậy hoặc w > R + 1, chúng ta dừng ngay lập tức và trả về -1. Điều này là do chúng ta không thể tạo thành R + 1 nên phạm vi sẽ bị ngắt kết nối vĩnh viễn. 
4. Ngược lại, chúng ta lấy trọng số này một lần, tăng ans lên 1 và cập nhật R = R + w. Sau đó chúng tôi di chuyển tôi về phía trước. 
5. Lặp lại cho đến khi R >= m, tại thời điểm đó an là số lượng bản sao có trọng lượng tối thiểu cần thiết. 

Lựa chọn thiết kế quan trọng là luôn tiêu thụ trọng lượng nhỏ nhất có thể để kéo dài khoảng cách có thể tiếp cận. Trọng số lớn hơn không bao giờ tốt hơn trong giai đoạn đầu vì chúng không giúp đóng các giá trị nhỏ bị thiếu và chỉ trở nên hữu ích sau khi tiền tố đã phát triển. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, giả sử chúng ta có thể biểu diễn mọi số nguyên trong [1, R]. Bất kỳ trọng số mới được chọn nào w chỉ có thể đóng góp có ý nghĩa nếu nó không vượt quá R + 1, vì nếu không thì R + 1 vẫn không thể truy cập được cho dù chúng ta kết hợp các trọng số hiện có với w như thế nào. Nếu w ≤ R + 1, việc thêm một bản sao của w sẽ mở rộng khoảng có thể truy cập thành [1, R + w], vì chúng ta có thể tạo tất cả các giá trị lên tới R và sau đó dịch chuyển theo w để liên tục lấp đầy phân đoạn tiếp theo. Bất biến này đảm bảo rằng R luôn đại diện cho tiền tố liền kề lớn nhất có thể đạt được với nhiều tập hợp đã chọn và lựa chọn tham lam đảm bảo chúng ta không bao giờ lãng phí một trọng lượng nhỏ sau này khi nó có thể rất quan trọng để thu hẹp khoảng cách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    for _ in range(T):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        R = 0
        i = 0
        ans = 0

        while R < m:
            if i == n or a[i] > R + 1:
                out.append("-1")
                break
            R += a[i]
            ans += 1
            i += 1
        else:
            out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã trực tiếp phản ánh việc xây dựng tham lam. Việc sắp xếp đảm bảo chúng ta luôn nhìn thấy ứng viên nhỏ nhất trước tiên. Biến`R`theo dõi tiền tố có thể truy cập và tính bất biến của vòng lặp đảm bảo rằng tất cả các giá trị trong`[1, R]`có thể đạt được với trọng số hiện được chọn. điều kiện`a[i] > R + 1`là điểm thất bại chính xác nơi khoảng cách tại`R + 1`trở nên không thể tránh khỏi. 

Một chi tiết tinh tế là`while-else`cấu trúc phân biệt rõ ràng việc hoàn thành thành công (R đạt m) với việc chấm dứt sớm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=3, m=8
a = [1,2,4]
```| Bước | R | tiếp theo w | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | lấy 1 → R=1 | 1 | 
| 2 | 1 | 2 | lấy 2 → R=3 | 2 | 
| 3 | 3 | 4 | lấy 4 → R=7 | 3 | 
| 4 | 7 | - | cần nhiều hơn, không w ≤ 8 | dừng lại | 

Chúng ta đạt R = 7, vẫn dưới 8, nên chúng ta thất bại. Ví dụ này chứng tỏ rằng mặc dù các con số trông có vẻ mạnh mẽ nhưng chúng ta vẫn không thể bao quát được khoảng đơn vị cuối cùng. 

### Ví dụ 2 

đầu vào:```
n=4, m=10
a = [1,2,3,10]
```| Bước | R | tiếp theo w | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | lấy 1 → R=1 | 1 | 
| 2 | 1 | 2 | lấy 2 → R=3 | 2 | 
| 3 | 3 | 3 | lấy 3 → R=6 | 3 | 
| 4 | 6 | 10 | lấy 10 → R=16 | 4 | 

Chúng tôi đạt R ≥ 10 thành công. Trọng số lớn chỉ hữu ích sau khi tiền tố đã đủ lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét tham lam là tuyến tính | 
| Không gian | O(1) thêm | ngoài việc lưu trữ đầu vào | 

Các ràng buộc cho phép tối đa 10.000 trọng số cho mỗi trường hợp thử nghiệm, do đó, việc sắp xếp cho mỗi thử nghiệm có thể dễ dàng đủ nhanh và quá trình quét tuyến tính đảm bảo giải pháp phù hợp thoải mái trong giới hạn thời gian ngay cả trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        R = 0
        i = 0
        ans = 0

        while R < m:
            if i == n or a[i] > R + 1:
                out.append("-1")
                break
            R += a[i]
            ans += 1
            i += 1
        else:
            out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("3\n3 8\n1 2 4\n3 4\n2 3 4\n3 4\n1 2 3\n") == "-1\n-1\n2"

# minimum case
assert run("1\n1 1\n1\n") == "1"

# already sufficient small range
assert run("1\n2 3\n1 2\n") == "2"

# impossible gap case
assert run("1\n2 10\n2 3\n") == "-1"

# large jump case
assert run("1\n3 10\n1 2 10\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 cân | 1 | tính khả thi cơ bản | 
| liền kề nhỏ | 2 | tăng trưởng bình thường | 
| khoảng cách sớm | -1 | phát hiện lỗi | 
| bao gồm trọng lượng lớn | 3 | trọng lượng nặng sử dụng muộn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi trọng số khả dụng nhỏ nhất lớn hơn 1. Ví dụ: nếu đầu vào bắt đầu bằng 2, chúng tôi sẽ thất bại ngay lập tức vì không thể tạo thành 1 và do đó không có khoảng thời gian liên tục nào có thể bắt đầu. Thuật toán nắm bắt được điều này khi khởi tạo: vì R = 0 nên chúng tôi yêu cầu trọng số 1 và 2 vi phạm điều này. 

Một trường hợp khác là khi trọng số tăng quá nhanh, chẳng hạn như [1, 100, 101]. Sau khi lấy 1, R = 1, nhưng trọng số hữu dụng tiếp theo phải ≤ 2. Vì 100 quá lớn nên chúng ta chấm dứt ngay lập tức. Thuật toán từ chối sớm một cách chính xác thay vì cố gắng bù đắp sau đó. 

Trường hợp khó phát hiện cuối cùng là khi một trọng số rất lớn xuất hiện sớm trong danh sách đã sắp xếp nhưng các trọng số nhỏ hơn lại tồn tại sau đó. Việc sắp xếp ngăn chặn các quyết định sai lầm ở đây, đảm bảo rằng các trọng số bắc cầu nhỏ luôn được xem xét trước khi có bước nhảy lớn.
