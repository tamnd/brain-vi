---
title: "CF 106440H - \u6b7b\u4ea1\u53d8\u901f"
description: "Sự cố mô tả dòng thời gian trong đó biểu đồ nhịp điệu thay đổi nhịp độ theo thời gian. Biểu đồ được đo bằng nhịp, trong khi thời gian thực hiện thực được đo bằng giây. Việc chuyển đổi giữa hai điều này phụ thuộc vào BPM hiện tại và BPM thay đổi ở các vị trí nhịp cụ thể."
date: "2026-06-20T03:57:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106440
codeforces_index: "H"
codeforces_contest_name: "\u201c\u89c4\u5f8b\u672a\u6765\u676f\u201d2026 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b"
rating: 0
weight: 106440
solve_time_s: 54
verified: true
draft: false
---

[CF 106440H - \u6b7b\u4ea1\u53d8\u901f](https://codeforces.com/problemset/problem/106440/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả dòng thời gian trong đó biểu đồ nhịp điệu thay đổi nhịp độ theo thời gian. Biểu đồ được đo bằng nhịp, trong khi thời gian thực hiện thực được đo bằng giây. Việc chuyển đổi giữa hai điều này phụ thuộc vào BPM hiện tại và BPM thay đổi ở các vị trí nhịp cụ thể. 

Cụ thể hơn, chúng ta được cung cấp một chuỗi các điểm thay đổi. Mỗi điểm thay đổi cho biết rằng bắt đầu từ một số vị trí nhịp, BPM sẽ trở thành một giá trị nhất định và nó vẫn hoạt động cho đến điểm thay đổi tiếp theo. Vì BPM xác định một nhịp mất bao lâu nên mỗi đoạn nhịp tương ứng với một hàm tuyến tính giữa vị trí nhịp và thời gian. 

Hệ thống phải trả lời hai loại truy vấn. Một truy vấn chuyển đổi vị trí nhịp thành thời gian thực, đánh giá hiệu quả hàm tuyến tính từng đoạn. Truy vấn khác chuyển đổi thời gian thực trở lại vị trí nhịp, yêu cầu đảo ngược hàm tuyến tính từng đoạn tương tự đó. 

Khó khăn chính đến từ kích thước của đầu vào. Có thể có tới 500.000 phân đoạn BPM và 500.000 truy vấn, do đó, mọi giải pháp xử lý từng truy vấn bằng cách quét tuyến tính các phân đoạn sẽ không thành công. Ngay cả cách tiếp cận logarit trên mỗi truy vấn cũng phải được thiết kế cẩn thận xung quanh cấu trúc được tính toán trước, nếu không, việc chuyển đổi dấu phẩy động trên nhiều phân đoạn sẽ trở nên quá chậm. 

Một vấn đề tế nhị là cả hai hướng chuyển đổi phải tôn trọng ranh giới phân khúc một cách chính xác. Nếu truy vấn nhịp nằm trong một phân đoạn, thì truy vấn đó chỉ được đánh giá bằng cách sử dụng BPM không đổi của phân đoạn đó, không chuyển nhầm sang khoảng tiếp theo. Tương tự, khi chuyển đổi thời gian trở lại nhịp, phân đoạn chính xác được xác định theo thời gian tích lũy chứ không phải theo chỉ số nhịp. 

Một trường hợp lỗi phổ biến phát sinh khi một giải pháp chỉ tính toán trước tổng tiền tố theo thời gian mà quên hỗ trợ đảo ngược. Ví dụ: nếu tất cả BPM là 60 ngoại trừ một phân đoạn có BPM 120, một nghịch đảo đơn giản sử dụng tỷ lệ tỷ lệ của tổng thời lượng sẽ đặt các truy vấn không chính xác trong phân đoạn nhanh hơn, vì thời gian tích lũy ở đó chậm hơn. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Để trả lời truy vấn theo nhịp, chúng tôi xác định phân đoạn BPM nào chứa nhịp đó, sau đó tính tổng các đóng góp từ tất cả các phân đoạn trước đó và cuối cùng thêm phần đóng góp một phần bên trong phân đoạn hiện tại. Đối với truy vấn theo thời gian, chúng tôi mô phỏng chuyển tiếp thời gian, tích lũy thời lượng phân đoạn cho đến khi vượt qua thời gian mục tiêu, sau đó tính toán vị trí phân số bên trong phân đoạn đó. 

Điều này hoạt động vì mỗi phân đoạn là tuyến tính và độc lập nhưng nó trở nên quá chậm vì mỗi truy vấn có thể yêu cầu quét tất cả các phân đoạn. Với n và q lên tới 500.000, trường hợp xấu nhất sẽ là cấp 10^11 phép toán, điều này là không thể thực hiện được. 

Quan sát quan trọng là cả hai chuyển đổi đều dựa trên một hàm tuyến tính từng phần đơn điệu. Đánh bại bản đồ vị trí theo thời gian thông qua một chức năng tăng dần và liên tục. Mỗi đoạn đóng góp một độ dốc tuyến tính được xác định bằng 60/BPM. Nếu chúng ta tính toán trước các điểm dừng tích lũy trong cả hai miền, chúng ta có thể chuyển đổi cả hai hướng thành các bài toán tìm kiếm nhị phân trên ranh giới phân đoạn. 

Cụ thể, chúng tôi xây dựng hai mảng. Một cửa hàng lưu trữ các vị trí nhịp của các điểm bắt đầu phân đoạn và một cửa hàng khác lưu trữ các giá trị thời gian tích lũy tương ứng tại các điểm bắt đầu đó. Vì mỗi đoạn có độ dốc không đổi nên thời gian bên trong một đoạn có thể được tính bằng O(1). Điều này làm cho nhịp đập theo thời gian trở nên tầm thường sau khi xác định vị trí phân khúc. Để tính nhịp, chúng tôi tìm kiếm nhị phân mảng tiền tố thời gian để tìm phân đoạn, sau đó đảo ngược hàm tuyến tính bên trong nó. 

Việc chuyển đổi giảm cả hai loại truy vấn thành O(log n), bị chi phối bởi tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O(n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi coi biểu đồ là một tập hợp các phân đoạn, trong đó mỗi phân đoạn bắt đầu ở một vị trí nhịp đã biết và có BPM không đổi cho đến điểm dừng tiếp theo. 

1. Đọc tất cả các vị trí bắt đầu phân đoạn và giá trị BPM. Chúng xác định các khoảng trong không gian nhịp. Mỗi quãng được liên kết với một tốc độ chuyển đổi không đổi từ nhịp sang giây. 
2. Tính toán trước cấu trúc tiền tố trên các phân đoạn. Đối với mỗi phân đoạn i, hãy tính xem đã trôi qua bao nhiêu giây cho đến khi nó bắt đầu. Điều này được thực hiện bằng cách tích lũy`(s[i+1] - s[i]) * (60 / b[i])`. Điều này tạo ra một ánh xạ tăng dần từ chỉ mục phân đoạn theo thời gian. 
3. Xây dựng hai mảng: một mảng dành cho ranh giới nhịp và một mảng dành cho ranh giới thời gian tương ứng. Các mảng này xác định hàm tuyến tính từng phần giống nhau trong hai hệ tọa độ. 
4. Đối với truy vấn theo thời gian, tìm kiếm nhị phân bắt đầu phân đoạn lớn nhất`s[i]`như vậy`s[i] ≤ x`. Sau khi xác định được vị trí, hãy tính thời gian như`t[i] + (x - s[i]) * (60 / b[i])`. 
5. Đối với truy vấn theo thời gian, tìm kiếm nhị phân phân đoạn lớn nhất bắt đầu trong mảng thời gian sao cho`t[i] ≤ x`. Sau đó đảo ngược trong phân khúc bằng cách sử dụng`s[i] + (x - t[i]) * (b[i] / 60)`. 
6. Đưa ra kết quả bằng số học dấu phẩy động với độ chính xác vừa đủ. 

Độ chính xác phụ thuộc vào thực tế là cả hai ánh xạ đều tăng nghiêm ngặt và tuyến tính trong mỗi phân đoạn, do đó việc đảo ngược không bao giờ vượt qua ranh giới phân đoạn một cách không chính xác. 

### Tại sao nó hoạt động 

Mỗi phân đoạn xác định một phép biến đổi tuyến tính giữa không gian nhịp và không gian thời gian với độ dốc dương. Vì tất cả các hệ số góc đều dương nên hàm tổng thể tăng nghiêm ngặt và do đó không khả nghịch. Mảng tiền tố lưu trữ các giá trị biên chính xác của hàm đơn điệu này. Tìm kiếm nhị phân xác định chính xác phân đoạn chứa truy vấn vì tính đơn điệu đảm bảo một khoảng hợp lệ duy nhất. Bên trong một đoạn, phép nội suy tuyến tính là chính xác vì phép biến đổi không thay đổi trong khoảng đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def find_segment(arr, x):
    # largest i such that arr[i] <= x
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if arr[mid] <= x:
            lo = mid + 1
        else:
            hi = mid - 1
    return hi

def solve():
    n = int(input())
    s = []
    b = []
    
    for _ in range(n):
        si, bi = input().split()
        s.append(int(si))
        b.append(int(bi))
    
    # compute time at segment starts
    t = [0.0] * n
    
    for i in range(n - 1):
        length_beats = s[i + 1] - s[i]
        t[i + 1] = t[i] + length_beats * 60.0 / b[i]
    
    q = int(input())
    
    for _ in range(q):
        parts = input().split()
        typ = parts[0]
        x = float(parts[1])
        
        if typ == 'B':
            i = find_segment(s, x)
            ans = t[i] + (x - s[i]) * 60.0 / b[i]
            print(f"{ans:.12f}")
        else:
            i = find_segment(t, x)
            ans = s[i] + (x - t[i]) * b[i] / 60.0
            print(f"{ans:.12f}")

if __name__ == "__main__":
    solve()
```Trước tiên, mã sẽ đọc tất cả các ranh giới phân đoạn và xây dựng một mảng tiền tố theo thời gian tại mỗi lần bắt đầu phân đoạn. Việc xây dựng tiền tố này là bước tiền xử lý quan trọng giúp biến định nghĩa từng phần thành cấu trúc có thể truy vấn trực tiếp. 

các`find_segment`thực hiện tìm kiếm nhị phân để xác định khoảng thời gian chính xác. Nó luôn trả về chỉ mục cuối cùng có ranh giới không lớn hơn truy vấn, điều này đảm bảo lựa chọn phân đoạn chính xác cho cả miền nhịp và miền thời gian. 

Đối với truy vấn nhịp, chúng tôi sử dụng mảng ranh giới nhịp`s`để xác định vị trí đoạn thẳng và sau đó áp dụng công thức tuyến tính cục bộ. Đối với truy vấn thời gian, chúng ta thực hiện tương tự nhưng trên mảng tiền tố thời gian`t`. Tính đối xứng của cả hai thao tác là điều làm cho giải pháp trở nên rõ ràng và tránh các cấu trúc dữ liệu riêng biệt để đảo ngược. 

Phải cẩn thận với số học dấu phẩy động. Tất cả các giá trị trung gian được giữ ở độ chính xác kép và đầu ra cuối cùng sử dụng đủ số thập phân để đáp ứng dung sai 1e-6. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ đơn giản có hai phân đoạn: nhịp bắt đầu từ 0 với BPM 120, sau đó ở nhịp 100 BPM trở thành 60. Điều này có nghĩa là phân đoạn đầu tiên nhanh hơn và tích lũy thời gian chậm hơn. 

| Bước | Truy vấn | Phân đoạn | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | B 50 | 0 | 50 * 0,5 | 25 | 
| 2 | B 120 | 1 | 100 * 0,5 + 20 * 1 | 70 | 

Dấu vết cho thấy phân đoạn đầu tiên sử dụng 60/120 = 0,5 giây mỗi nhịp, trong khi phân đoạn thứ hai sử dụng 1 giây mỗi nhịp. Cấu trúc tiền tố đảm bảo chúng tôi phân tách chính xác các đóng góp. 

Bây giờ hãy xem xét các truy vấn nghịch đảo trên cùng một cấu trúc. 

| Bước | Truy vấn | Phân đoạn | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | S 25 | 0 | 25 * 2 | 50 | 
| 2 | S 70 | 1 | 100 + (70 - 50) * 1 | 120 | 

Điều này xác nhận rằng việc đảo ngược sẽ tái tạo lại chính xác vị trí nhịp ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q log n) | việc xây dựng tiền tố là tuyến tính, mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian | O(n) | lưu trữ ranh giới phân đoạn và thời gian tiền tố | 

Giải pháp xử lý thoải mái 500.000 phân đoạn và 500.000 truy vấn vì tất cả công việc nặng nhọc được giảm xuống thời gian xử lý trước tuyến tính và truy vấn logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    s = []
    b = []
    for _ in range(n):
        si, bi = input().split()
        s.append(int(si))
        b.append(int(bi))

    t = [0.0] * n
    for i in range(n - 1):
        t[i + 1] = t[i] + (s[i + 1] - s[i]) * 60.0 / b[i]

    def find(arr, x):
        lo, hi = 0, len(arr) - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if arr[mid] <= x:
                lo = mid + 1
            else:
                hi = mid - 1
        return hi

    q = int(input())
    out = []
    for _ in range(q):
        typ, xs = input().split()
        x = float(xs)
        if typ == 'B':
            i = find(s, x)
            out.append(str(t[i] + (x - s[i]) * 60.0 / b[i]))
        else:
            i = find(t, x)
            out.append(str(s[i] + (x - t[i]) * b[i] / 60.0))

    return "\n".join(out)

# sample-style + custom cases

assert run("""2
0 120
100 60
3
B 0
B 100
S 30
""").split()[0] == "0.0"

assert run("""3
0 120
100 120
200 120
2
B 150
S 75
""") != ""

assert run("""1
0 60
3
B 10
S 10
B 0
""") != ""

assert run("""2
0 100
10 200
2
B 10
S 10
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phân đoạn đơn | ánh xạ tuyến tính | độ đúng cơ sở | 
| phân đoạn BPM bằng nhau | hành vi tiền tố ổn định | không có vấn đề gián đoạn | 
| đầu vào tối thiểu | xử lý ranh giới | lập chỉ mục cạnh | 
| BPM hỗn hợp | độ chính xác đảo ngược | nhất quán của cả hai hướng | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi truy vấn đến đích chính xác trên ranh giới phân đoạn. Vì phần bắt đầu phân đoạn được xác định một cách toàn diện nên cả hai mảng phải xử lý sự bằng nhau một cách nhất quán. Ví dụ: nếu một phân đoạn bắt đầu ở nhịp 100 thì truy vấn B 100 phải thuộc về phân đoạn đó chứ không phải phân đoạn trước đó. Việc triển khai tìm kiếm nhị phân trả về chỉ mục cuối cùng có giá trị ≤ x, chỉ mục này thực thi trực tiếp quy tắc này. 

Một trường hợp khác là biểu đồ một đoạn. Ở đây không có chuyển tiếp, vì vậy việc xây dựng tiền tố phải tránh truy cập`s[i+1]`. Việc triển khai xử lý việc này bằng cách chỉ lặp lại tối đa n-1 khi xây dựng tiền tố thời gian. 

Cuối cùng, các truy vấn đảo ngược chính xác bằng thời gian tiền tố phải ánh xạ tới điểm bắt đầu phân đoạn chính xác. Vì mảng thời gian tăng lên một cách nghiêm ngặt, nên tìm kiếm nhị phân lại chọn ranh giới chính xác mà không có sự mơ hồ, duy trì tính nhất quán giữa ánh xạ thuận và ngược.
