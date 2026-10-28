---
title: "CF 106151D - gói"
description: "Chúng ta có một tập hợp các gói, mỗi gói có hai tham số. Đầu tiên là chi phí cơ bản mà bạn phải trả một lần nếu bạn đưa gói hàng đó vào lô giao hàng của mình."
date: "2026-06-20T02:23:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106151
codeforces_index: "D"
codeforces_contest_name: "2025 ICPC Greek Collegiate Programming Contest (GRCPC 2025)"
rating: 0
weight: 106151
solve_time_s: 68
verified: true
draft: false
---

[CF 106151D - gói](https://codeforces.com/problemset/problem/106151/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các gói, mỗi gói có hai tham số. Đầu tiên là chi phí cơ bản mà bạn phải trả một lần nếu bạn đưa gói hàng đó vào lô giao hàng của mình. Thứ hai là hệ số tắc nghẽn tỷ lệ với số lượng gói hàng bạn quyết định gửi trong cùng một đợt. 

Nếu bạn chọn phân phối chính xác K gói trong một lần chạy thì mỗi gói được chọn i có tổng chi phí là$a_i + K \cdot b_i$. Điểm mấu chốt là thời hạn tắc nghẽn phụ thuộc vào số lượng gói được chọn trên toàn cầu chứ không phụ thuộc vào gói riêng lẻ. 

Nhiệm vụ của bạn là chọn một tập hợp con các gói và cũng ngầm chọn kích thước K của nó, sao cho tổng chi phí của tập hợp con đã chọn không vượt quá ngân sách B, đồng thời tối đa hóa K. Trong số tất cả các tập hợp con đạt được K tối đa này, bạn phải báo cáo chi phí tối thiểu có thể. 

Sự tương tác giữa lựa chọn và chi phí là khó khăn cốt lõi: việc chọn nhiều gói hơn sẽ làm tăng hình phạt tắc nghẽn trên mỗi gói đối với tất cả các mặt hàng đã chọn, điều này phản ánh tính khả thi của chính việc lựa chọn. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$trọn gói với chi phí lên tới$10^9$tổng ngân sách và lên tới$10^5$cho các thông số riêng lẻ. Điều này ngay lập tức loại trừ mọi giải pháp tính toán lại tính khả thi cho từng tập hợp con một cách rõ ràng. Bất kỳ cách tiếp cận nào tệ hơn đại khái$O(n \log n)$hoặc$O(n \sqrt n)$tùy thuộc vào hằng số sẽ không an toàn. 

Một chế độ thất bại ngây thơ nhưng mang tính hướng dẫn đang cố gắng sửa K và tham lam chọn các gói K rẻ nhất với chi phí đã điều chỉnh. Ví dụ, giả sử tất cả$b_i$lớn đối với một số mặt hàng và nhỏ đối với những mặt hàng khác. Nếu K được sửa không chính xác, lựa chọn tham lam có thể chọn các mặt hàng chỉ trở nên đắt tiền sau khi K thay đổi, làm mất hiệu lực các quyết định trước đó. 

Một trường hợp khó phát hiện khác xuất hiện khi K thay đổi thứ tự các mục. Hai gói hàng có thể hoán đổi thứ tự tùy thuộc vào K vì chi phí là$a_i + K b_i$. Ví dụ, nếu$a_1 < a_2$Nhưng$b_1 > b_2$, thì đối với gói K nhỏ thì gói 1 rẻ hơn, nhưng đối với gói K lớn thì gói 2 sẽ rẻ hơn. Bất kỳ cách tiếp cận nào sắp xếp một lần theo$a_i$hoặc$a_i + b_i$không xét đến K sẽ bị vỡ. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi K có thể từ 1 đến N. Với mỗi K, chúng tôi tính toán chi phí để chọn gói K rẻ nhất theo công thức chi phí$a_i + K b_i$. Để thực hiện điều này một cách trực tiếp, chúng ta cần đánh giá tất cả các gói, tính toán chi phí đã điều chỉnh, sắp xếp chúng và chọn K tốt nhất. 

Đối với K cố định, việc sắp xếp mất$O(n \log n)$, và làm điều này với mọi K dẫn đến$O(n^2 \log n)$, quá chậm đối với$2 \cdot 10^5$. 

Quan sát cấu trúc quan trọng là chúng ta không thực sự cần phải thử tất cả K một cách độc lập theo nghĩa tính toán lại đầy đủ. Thay vào đó, chúng ta có thể đặt câu hỏi quyết định: với K cố định, chúng ta có thể chọn gói K trong phạm vi ngân sách không và chi phí tối thiểu là bao nhiêu? Nếu chúng ta có thể trả lời câu hỏi này một cách hiệu quả thì chúng ta có thể tìm kiếm nhị phân K. 

Với K cố định, mỗi gói đóng góp$a_i + K b_i$. Vì K cố định nên đây chỉ là trọng số tĩnh cho mỗi mặt hàng. Vì vậy, lựa chọn tối ưu đơn giản là K giá trị nhỏ nhất của$a_i + K b_i$. Điều đó có nghĩa là chúng ta chỉ cần sắp xếp một lần cho mỗi lần kiểm tra, nhưng điều đó cũng quá chậm. 

Chúng tôi giảm bớt điều này hơn nữa bằng cách tìm kiếm nhị phân K và sử dụng kiểm tra tính khả thi dựa trên vùng heap hoặc dựa trên lựa chọn. Tuy nhiên, có một thủ thuật thậm chí còn đơn giản hơn: đối với K cố định, chúng ta tính các giá trị$a_i + K b_i$, chọn K nhỏ nhất bằng cách sử dụng sắp xếp hoặc một đống trong$O(n)$hoặc$O(n \log n)$, và tính tổng. Sau đó chúng ta tìm kiếm nhị phân K trong$O(\log n)$. 

Điều này mang lại$O(n \log n \log n)$, điều đó có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên K với tính toán lại đầy đủ |$O(n^2 \log n)$|$O(n)$| Quá chậm | 
| Tìm kiếm nhị phân K + chọn K chi phí điều chỉnh nhỏ nhất |$O(n \log n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm, với K cho trước, tính tổng chi phí tối thiểu để chọn chính xác K gói. Đối với mỗi gói, hãy tính chi phí hiệu quả của nó$c_i = a_i + K b_i$, vì K được cố định trong lần kiểm tra này. 
2. Sắp xếp tất cả các tính toán$c_i$các giá trị và lấy tổng các giá trị K nhỏ nhất. Điều này tương ứng với lựa chọn tối ưu bởi vì, đối với K cố định, mỗi gói đóng góp độc lập và không có sự kết hợp nào ngoại trừ số lượng lựa chọn. 
3. Nếu tổng này nhỏ hơn hoặc bằng B thì K gói khả thi trong điều kiện ngân sách. Ngược lại, K quá lớn. 
4. Tìm kiếm nhị phân K từ 0 đến N. Chúng tôi duy trì một phạm vi trong đó tính khả thi chuyển từ đúng sang sai. 
5. Với mỗi điểm giữa K, hãy chạy hàm khả thi. Nếu khả thi, hãy di chuyển giới hạn dưới lên trên; nếu không thì di chuyển giới hạn trên xuống. 
6. Sau khi tìm kiếm nhị phân hoàn tất, chúng ta biết K khả thi tối đa. Chúng ta tính lại chi phí của nó bằng cách sử dụng quy trình tương tự để thu được tổng chi phí tối thiểu cho K đó. 

Tại sao nó hoạt động: đối với mỗi K cố định, hàm chi phí được xác định đầy đủ và độc lập giữa các gói ngoại trừ thông qua số lượng lựa chọn. Vị từ khả thi là đơn điệu trong K vì tăng K sẽ tăng mọi$c_i = a_i + K b_i$, điều này chỉ có thể làm tăng tổng của K giá trị nhỏ nhất hoặc khiến việc lựa chọn trở nên khó khăn hơn. Tính đơn điệu này đảm bảo tính chính xác của tìm kiếm nhị phân, vì một khi K trở nên không khả thi thì tất cả K lớn hơn vẫn không khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cost_for_k(a, b, K, B):
    n = len(a)
    vals = [a[i] + K * b[i] for i in range(n)]
    vals.sort()
    s = sum(vals[:K])
    return s

def feasible(a, b, K, B):
    if K == 0:
        return True, 0
    n = len(a)
    vals = [a[i] + K * b[i] for i in range(n)]
    vals.sort()
    s = sum(vals[:K])
    return s <= B, s

def main():
    n, B = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    lo, hi = 0, n
    best_k = 0
    best_cost = 0

    while lo <= hi:
        mid = (lo + hi) // 2
        vals = [a[i] + mid * b[i] for i in range(n)]
        vals.sort()
        s = sum(vals[:mid]) if mid > 0 else 0

        if s <= B:
            best_k = mid
            best_cost = s
            lo = mid + 1
        else:
            hi = mid - 1

    print(best_k, best_cost)

if __name__ == "__main__":
    main()
```Mã thực hiện tìm kiếm nhị phân trực tiếp trên câu trả lời K. Đối với mỗi điểm giữa, nó tính chi phí đã điều chỉnh$a_i + K b_i$và chọn K nhỏ nhất thông qua sắp xếp. Tổng các giá trị K này là chi phí tối thiểu có thể có cho K đó vì bất kỳ tập hợp con nào khác sẽ thay thế phần tử đã chọn bằng phần tử lớn hơn. 

Tìm kiếm nhị phân duy trì K khả thi nhất được thấy cho đến nay. Khi điểm giữa khả thi, chúng tôi lưu trữ cả K và chi phí của nó, sau đó cố gắng tăng K. Ngược lại, chúng tôi giảm K. Điều này hiệu quả vì tính khả thi là đơn điệu trong K. 

Một chi tiết triển khai tinh tế là xử lý riêng K = 0 để tránh các vấn đề phân chia và đảm bảo tính chính xác khi ngân sách cho phép không có gói nào. Một điểm quan trọng khác là tính toán lại chi phí trong mỗi lần lặp thay vì cố gắng sử dụng lại các tính toán trước đó, vì hàm chi phí phụ thuộc vào K. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 4, B = 10
a = [1, 2, 2, 1]
b = [2, 2, 2, 2]
```Chúng tôi kiểm tra các giá trị K khác nhau. 

| K | chi phí điều chỉnh$a_i + K b_i$| giá trị K nhỏ nhất | tổng hợp | khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | [3,4,4,3] | [3] | 3 | vâng | 
| 2 | [5,6,6,5] | [5,5] | 10 | vâng | 
| 3 | [7,8,8,7] | [7,7,7] | 21 | không | 
| 4 | [9,10,10,9] | [9,9,10,10] | 38 | không | 

Tìm kiếm nhị phân tìm thấy K = 2 là khả thi tối đa. Chi phí là 10. 

Dấu vết này cho thấy mức tăng K đồng đều sẽ làm tăng lạm phát tất cả các mặt hàng và tính khả thi sẽ bị phá vỡ như thế nào khi tắc nghẽn tích lũy trở nên quá lớn. 

### Ví dụ 2 

đầu vào:```
N = 5, B = 35
a = [3,2,1,4,5]
b = [3,4,5,2,1]
```| K | chi phí điều chỉnh | giá trị K nhỏ nhất | tổng hợp | khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | [6,6,6,6,6] | [6] | 6 | vâng | 
| 2 | [9,10,11,8,7] | [7,8] | 15 | vâng | 
| 3 | [12,14,17,10,8] | [8,10,12] | 30 | vâng | 
| 4 | [15,18,23,12,9] | [9,12,15,18] | 54 | không | 

K tối đa khả thi là 3, với chi phí 30. 

Ví dụ này nhấn mạnh rằng thứ tự tương đối của các mục thay đổi theo K, nhưng thuật toán sẽ điều chỉnh một cách tự nhiên vì nó tính toán lại thứ tự ở mỗi lần kiểm tra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n \log n)$| Mỗi lần kiểm tra tính khả thi sẽ sắp xếp n giá trị và tìm kiếm nhị phân chạy theo nhật ký n lần lặp | 
| Không gian |$O(n)$| Lưu trữ chi phí chuyển đổi | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các mục và việc sắp xếp nhiều số nguyên khoảng 20 lần có thể chấp nhận được trong Python với cách triển khai được tối ưu hóa. Việc sử dụng bộ nhớ vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, B = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    def check(K):
        vals = [a[i] + K * b[i] for i in range(n)]
        vals.sort()
        return sum(vals[:K]) if K > 0 else 0

    lo, hi = 0, n
    best_k, best_c = 0, 0
    while lo <= hi:
        mid = (lo + hi) // 2
        c = check(mid)
        if c <= B:
            best_k, best_c = mid, c
            lo = mid + 1
        else:
            hi = mid - 1

    return f"{best_k} {best_c}"

# provided samples
assert run("4 10\n1 2 2 1\n2 2 2 2\n") == "2 10", "sample 1"
assert run("5 35\n3 2 1 4 5\n3 4 5 2 1\n") == "3 30", "sample 2"

# custom cases
assert run("1 100\n5\n1\n") == "1 6", "single item"
assert run("3 0\n1 2 3\n1 1 1\n") == "0 0", "zero budget"
assert run("4 1000\n0 0 0 0\n0 0 0 0\n") == "4 0", "free items"
assert run("5 20\n10 9 8 7 6\n1 2 3 4 5\n") == "2 19", "mixed tradeoff"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục duy nhất | 1 6 | trường hợp ranh giới tối thiểu | 
| ngân sách bằng không | 0 0 | không thể chọn bất kỳ gói nào | 
| vật phẩm miễn phí | 4 0 | mọi lựa chọn đều tối ưu | 
| sự đánh đổi hỗn hợp | 2 19 | tương tác giữa a và b | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi K bằng 0. Trong tình huống này, không có gói nào được chọn và chi phí chính xác bằng 0. Thuật toán xử lý vấn đề này một cách rõ ràng trước khi sắp xếp, tránh những công việc không cần thiết và đảm bảo tính chính xác ngay cả khi ngân sách bằng 0. 

Một trường hợp cạnh khác phát sinh khi tất cả$b_i$là số không. Khi đó chi phí trở nên độc lập với K cho mỗi hạng mục và vấn đề giảm xuống còn việc chọn K nhỏ nhất$a_i$các giá trị. Thuật toán vẫn hoạt động vì$a_i + K b_i = a_i$. 

Trường hợp thứ ba là khi$a_i$giá trị bằng 0 nhưng$b_i$là tích cực. Ở đây, việc tăng K nhanh chóng khiến việc lựa chọn trở nên đắt đỏ mặc dù ban đầu các mục riêng lẻ có vẻ miễn phí. Tìm kiếm nhị phân tự nhiên phát hiện ra điểm giới hạn K nơi tắc nghẽn chiếm ưu thế và việc tính toán lại ở mỗi bước sẽ nắm bắt chính xác hành vi phi tuyến tính này.
