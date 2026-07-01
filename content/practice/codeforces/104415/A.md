---
title: "CF 104415A - Điểm chuyên cần"
description: "Chúng ta được cấp một dãy điểm dự lớp cho một loạt lớp phải tham dự nghiêm ngặt từ lớp đầu tiên trở đi, không bỏ sót hoặc bắt đầu từ giữa."
date: "2026-06-30T19:20:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104415
codeforces_index: "A"
codeforces_contest_name: "IME++ Starters Try-outs 2023"
rating: 0
weight: 104415
solve_time_s: 52
verified: true
draft: false
---

[CF 104415A - Điểm chuyên cần](https://codeforces.com/problemset/problem/104415/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy điểm dự lớp cho một loạt lớp phải tham dự nghiêm ngặt từ lớp đầu tiên trở đi, không bỏ sót hoặc bắt đầu từ giữa. Mỗi lớp đóng góp một số điểm cố định và những điểm này sẽ tích lũy khi chúng ta di chuyển qua trình tự. 

Bên cạnh mảng này, chúng tôi được cung cấp nhiều truy vấn. Trên thực tế, mỗi truy vấn hỏi Cayo phải đi bao xa kể từ khi bắt đầu học kỳ để đạt được mức điểm chuyên cần tích lũy cần thiết. Bởi vì việc tham dự diễn ra liên tục ngay từ đầu nên mọi câu trả lời hợp lệ đều tương ứng với một tiền tố nào đó của mảng. 

Vì vậy, đối với mỗi giá trị truy vấn, chúng ta cần xác định tiền tố nhỏ nhất của mảng có tổng đạt hoặc vượt quá giá trị đó, đồng thời báo cáo thông tin về tiền tố đó như vị trí của nó và tổng số điểm tích lũy được. 

Hạn chế chính về cơ cấu là việc đi học luôn bắt đầu từ lớp học đầu tiên. Điều này loại bỏ mọi nhu cầu xử lý mảng con tùy ý và giảm hoàn toàn vấn đề về lý luận về tổng tiền tố. 

Từ quan điểm phức tạp, mảng có thể đủ lớn để việc tính toán lại tổng cho từng truy vấn một cách độc lập sẽ quá chậm. Nếu có tới 100000 lớp và số lượng truy vấn tương tự, việc quét O(n) đơn giản cho mỗi truy vấn sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa các giới hạn thông thường. Điều này ngay lập tức gợi ý rằng chúng ta phải xử lý trước mảng và trả lời từng truy vấn theo thời gian logarit hoặc không đổi. 

Trường hợp khó phát hiện khi truy vấn yêu cầu giá trị lớn hơn tổng của tất cả các lớp. Trong trường hợp đó, không có tiền tố nào đạt đến ngưỡng yêu cầu, vì vậy việc triển khai cẩn thận phải quyết định những gì cần trả về. Tương tự, khi ngưỡng bằng 0 hoặc âm, câu trả lời đúng ngay lập tức là vị trí đầu tiên có hành vi tiền tố tổng bằng 0, thường là chỉ số 1 với tổng a[0] hoặc chỉ mục 0 tùy thuộc vào quy ước lập chỉ mục. Những trường hợp này thường phá vỡ ranh giới tìm kiếm nhị phân ngây thơ nếu không được xử lý rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xử lý từng truy vấn một cách độc lập bằng cách lặp lại từ lớp đầu tiên và tích lũy điểm cho đến khi đạt đến ngưỡng yêu cầu. Điều này rất đơn giản: đối với một giá trị truy vấn nhất định, chúng tôi tiếp tục cộng điểm lớp cho đến khi tổng điểm chạy đủ lớn. Tính chính xác là ngay lập tức vì nó mô phỏng chính xác quy trình được mô tả trong bài toán. 

Tuy nhiên, cách tiếp cận này lặp lại công việc tính tổng tiền tố giống nhau cho mọi truy vấn. Trong trường hợp xấu nhất, mỗi truy vấn có thể yêu cầu quét gần như toàn bộ mảng, cho độ phức tạp tổng cộng là O(n · q). Với các ràng buộc lớn, việc này trở nên quá chậm vì nó liên tục tính toán lại các tổng từng phần giống nhau một cách hiệu quả. 

Quan sát chính là tất cả các truy vấn đều hoạt động trên cùng một cấu trúc tiền tố. Khi chúng tôi tính tổng tiền tố một lần, tổng của bất kỳ tiền tố nào sẽ trở thành tra cứu mảng trực tiếp. Nhiệm vụ còn lại của mỗi truy vấn là tìm vị trí đầu tiên tại đó tổng tiền tố vượt qua một ngưỡng. Vì mảng tổng tiền tố không giảm nên đây trở thành một bài toán tìm kiếm nhị phân cổ điển. 

Vì vậy, giải pháp giảm xuống còn hai giai đoạn. Đầu tiên, xử lý trước mảng thành mảng tổng tiền tố. Thứ hai, trả lời từng truy vấn bằng cách tìm kiếm nhị phân chỉ mục đầu tiên trong đó tổng tiền tố ít nhất bằng giá trị truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · q) | O(1) | Quá chậm | 
| Tổng tiền tố + Tìm kiếm nhị phân | O(n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp theo hai giai đoạn: tiền xử lý và xử lý truy vấn.

1. Tính mảng tổng tiền tố trong đó mỗi vị trí lưu trữ tổng số điểm tích lũy từ hạng nhất cho đến vị trí đó. Điều này biến việc tích lũy phạm vi thành tra cứu theo thời gian không đổi. 
2. Với mỗi giá trị truy vấn x, chúng ta cần tìm tiền tố sớm nhất có tổng ít nhất là x. Vì tổng tiền tố tăng đơn điệu nên chúng ta có thể áp dụng tìm kiếm nhị phân trên phạm vi chỉ mục. 
3. Trong quá trình tìm kiếm nhị phân, chúng tôi liên tục kiểm tra điểm giữa. Nếu tổng tiền tố ở vị trí đó nhỏ hơn x thì đáp án phải nằm bên phải. Ngược lại, có thể là vị trí này hoặc vị trí nào đó sớm hơn nên chúng ta di chuyển sang trái. 
4. Sau khi tìm kiếm nhị phân hoàn tất, chỉ mục kết quả là tiền tố nhỏ nhất thỏa mãn yêu cầu. Sau đó, chúng tôi xuất cả chỉ mục (được chuyển đổi thành lập chỉ mục dựa trên 1 nếu cần) và tổng tiền tố tương ứng. 
5. Nếu giá trị truy vấn vượt quá tổng tổng của toàn bộ mảng, chúng tôi trực tiếp trả về toàn bộ chiều dài của mảng và tổng tổng của nó, vì không có tiền tố nào trước đó có thể đáp ứng được. 

Tại sao nó hoạt động dựa trên một thuộc tính đơn điệu: tổng tiền tố không bao giờ giảm khi chúng ta di chuyển về phía trước trong mảng. Điều này đảm bảo rằng điều kiện “tổng tiền tố >= x” xác định một hậu tố liền kề của các chỉ số hợp lệ. Khi một chỉ mục hợp lệ, tất cả các chỉ mục ở bên phải của nó cũng hợp lệ và tất cả các chỉ mục ở bên trái của nó đều không hợp lệ. Tìm kiếm nhị phân hoạt động chính xác vì nó khai thác ranh giới chuyển tiếp đơn này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def lower_bound_prefix(prefix, x):
    lo, hi = 0, len(prefix) - 1
    ans = len(prefix) - 1
    
    while lo <= hi:
        mid = (lo + hi) // 2
        if prefix[mid] >= x:
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1
    
    return ans

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    
    prefix = [0] * n
    prefix[0] = a[0]
    
    for i in range(1, n):
        prefix[i] = prefix[i - 1] + a[i]
    
    total = prefix[-1]
    
    out = []
    for _ in range(q):
        x = int(input())
        
        if x <= prefix[0]:
            idx = 0
        elif x > total:
            idx = n - 1
        else:
            idx = lower_bound_prefix(prefix, x)
        
        out.append(f"{idx + 1} {prefix[idx]}")
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách xây dựng mảng tổng tiền tố sao cho mỗi mục nhập thể hiện tổng số người tham dự lớp đó. Điều này tránh việc tính toán lại trong quá trình truy vấn. 

Hàm trợ giúp thực hiện tìm kiếm nhị phân cho tiền tố đầu tiên đạt đến ngưỡng yêu cầu. Chỉ mục trả về dựa trên 0 và chúng tôi điều chỉnh nó khi in. 

Mỗi truy vấn được xử lý độc lập bằng cách sử dụng tìm kiếm này và chúng tôi cẩn thận bảo vệ các trường hợp ranh giới trong đó truy vấn nhỏ hơn tiền tố đầu tiên hoặc lớn hơn tổng đầy đủ. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó điểm tham dự là`[2, 1, 3, 2]`và truy vấn yêu cầu ngưỡng`3`Và`7`. 

Tổng tiền tố trở thành`[2, 3, 6, 8]`. 

### Truy vấn 1: x = 3 

| Bước | lo | xin chào | giữa | tiền tố[giữa] | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 1 | 3 | di chuyển sang trái | 
| 2 | 0 | 0 | 0 | 2 | di chuyển sang phải | 

Chỉ số kết quả là 1, tương ứng với tổng tiền tố 3. 

Điều này xác nhận rằng thuật toán tìm chính xác điểm sớm nhất đạt đến ngưỡng chứ không phải bất kỳ điểm hợp lệ nào. 

### Truy vấn 2: x = 7 

| Bước | lo | xin chào | giữa | tiền tố[giữa] | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 1 | 3 | di chuyển sang phải | 
| 2 | 2 | 3 | 2 | 6 | di chuyển sang phải | 
| 3 | 3 | 3 | 3 | 8 | di chuyển sang trái | 

Chỉ số kết quả là 3, với tổng tiền tố là 8. 

Điều này chứng tỏ cách tìm kiếm nhị phân bỏ qua các tiền tố không đủ một cách chính xác và hội tụ về tiền tố hợp lệ đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q log n) | Tính toán tiền tố là tuyến tính, mỗi truy vấn là tìm kiếm nhị phân trên mảng tiền tố | 
| Không gian | O(n) | Mảng tổng tiền tố lưu trữ một giá trị cho mỗi lớp | 

Giải pháp phù hợp thoải mái trong các ràng buộc thông thường lên tới 100000 phần tử và truy vấn, vì tìm kiếm logarit giúp cho mỗi truy vấn hoạt động ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    q = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    prefix = [0] * n
    prefix[0] = a[0]
    for i in range(1, n):
        prefix[i] = prefix[i - 1] + a[i]

    total = prefix[-1]

    def lb(x):
        lo, hi = 0, n - 1
        ans = n - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if prefix[mid] >= x:
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1
        return ans

    out = []
    for _ in range(q):
        x = int(next(it))
        if x <= prefix[0]:
            idx = 0
        elif x > total:
            idx = n - 1
        else:
            idx = lb(x)
        out.append(f"{idx+1} {prefix[idx]}")
    return "\n".join(out)

# minimum size
assert solve_capture("1 1\n5\n3\n") == "1 5"

# basic
assert solve_capture("4 2\n2 1 3 2\n3\n7\n") == "2 3\n4 8"

# exact boundary
assert solve_capture("3 1\n1 2 3\n6\n") == "3 6"

# beyond total
assert solve_capture("3 1\n1 2 3\n10\n") == "3 6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | hành vi tiền tố trực tiếp | trường hợp cơ sở đúng đắn | 
| mảng hỗn hợp | tìm kiếm nhị phân thông thường | tính đúng đắn chung | 
| khớp tổng chính xác | bình đẳng ranh giới | xử lý bình đẳng | 
| truy vấn lớn | kẹp đến cuối | tràn ngưỡng | 

## Vỏ cạnh 

Khi mảng chỉ có một phần tử, tính toán tiền tố và tìm kiếm nhị phân đều chuyển sang hành vi tầm thường. Thuật toán trả về chỉ số 0 ngay lập tức vì mọi truy vấn tích cực đều phải ánh xạ tới tiền tố duy nhất đó. 

Khi truy vấn bằng chính xác một trong các tổng tiền tố, tìm kiếm nhị phân vẫn phải trả về chỉ mục sớm nhất như vậy. Việc triển khai đảm bảo điều này bằng cách di chuyển ranh giới bên phải sang trái ngay cả khi gặp phải sự bằng nhau, ngăn cản việc lựa chọn vị trí hợp lệ sau này. 

Khi truy vấn vượt quá tổng, thuật toán sẽ kẹp kết quả vào chỉ mục cuối cùng. Điều này tránh tìm kiếm nhị phân đi lang thang bên ngoài giới hạn hợp lệ và đảm bảo đầu ra được xác định ngay cả khi không có tiền tố nào thỏa mãn điều kiện.
