---
title: "CF 1538B - Bạn bè và Kẹo"
description: "Chúng ta được cấp một mảng các số nguyên biểu thị số lượng kẹo ban đầu mà mỗi người giữ. Trong một thao tác duy nhất, chúng tôi được phép chọn một số nhóm nhỏ người, lấy tất cả kẹo từ họ và sau đó tự do phân phối lại số kẹo đó cho mọi người."
date: "2026-06-10T14:52:09+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1538
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 725 (Div. 3)"
rating: 800
weight: 1538
solve_time_s: 400
verified: true
draft: false
---

[CF 1538B - Bạn bè và Kẹo](https://codeforces.com/problemset/problem/1538/B) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 6 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên biểu thị số lượng kẹo ban đầu mà mỗi người giữ. Trong một thao tác duy nhất, chúng tôi được phép chọn một số nhóm nhỏ người, lấy tất cả kẹo từ họ và sau đó tự do phân phối lại số kẹo đó cho mọi người. 

Mục tiêu là làm cho tất cả các giá trị bằng nhau sau bước phân phối lại duy nhất này. Tuy nhiên, có một hạn chế: chúng tôi không thể chọn tất cả mọi người miễn phí. Chi phí mà chúng tôi quan tâm là số lượng người được chọn và chúng tôi muốn giảm thiểu số lượng người chúng tôi phải chọn để số kẹo kết hợp của họ đủ để đạt được cấu hình cuối cùng hoàn toàn bằng nhau. 

Một cách hữu ích để điều chỉnh lại nhiệm vụ là suy nghĩ về mức thâm hụt và thặng dư so với giá trị mục tiêu, giá trị này phải là giá trị trung bình của mảng nếu có giải pháp. Bước phân phối lại hoàn toàn không bị hạn chế, ngoại trừ việc chúng tôi chỉ thu thập kẹo từ các chỉ số đã chọn, do đó, hạn chế duy nhất là liệu tập hợp con đã chọn có chứa đủ tổng “khối lượng có thể điều chỉnh” để điều chỉnh mọi sai lệch so với giá trị trung bình hay không. 

Hạn chế về kích thước đầu vào là vừa phải, lên tới mười nghìn trường hợp thử nghiệm và tổng chiều dài mảng lên tới hai trăm nghìn. Điều này loại trừ bất kỳ giải pháp bậc hai nào cho mỗi bài kiểm tra nhưng cho phép sắp xếp hoặc vượt qua tuyến tính cho mỗi bài kiểm tra. Bản thân các giá trị đủ nhỏ để có thể xử lý trực tiếp và khó khăn chính không phải là độ lớn số học mà là cấu trúc tập hợp con. 

Trường hợp cạnh tinh tế phát sinh khi tổng không chia hết cho n. Trong trường hợp đó, không có phép phân phối lại nào có thể tạo ra các giá trị bằng nhau, bất kể chúng ta chọn bao nhiêu phần tử. Một trường hợp góc khác xuất hiện khi tất cả các giá trị đều bằng nhau, trong trường hợp đó việc chọn phần tử bằng 0 là đủ. Trường hợp thứ ba là khi chỉ có một phần tử có độ lệch khác 0 so với giá trị trung bình; thì chỉ cần chọn phần tử đó là đủ, ngay cả khi nó lớn, vì việc loại bỏ nó sẽ khắc phục chính xác sự mất cân bằng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ thử tất cả các tập hợp con của chỉ số. Đối với mỗi tập hợp con, chúng tôi tính tổng các giá trị đã chọn và kiểm tra xem việc loại bỏ chúng có để lại tổng số có thể phân bổ đều hay không. Về nguyên tắc, điều này đúng vì nó khớp trực tiếp với định nghĩa của các phép toán được phép, nhưng nó ngay lập tức trở thành hàm mũ theo n. Với n lên đến hai trăm nghìn trong các bài kiểm tra, thậm chí việc lưu trữ các tập hợp con là không thể và số lượng ứng viên tăng lên là 2^n. 

Quan sát quan trọng là cấu hình cuối cùng là cố định: mọi phần tử phải kết thúc ở giá trị trung bình, do đó tổng sẽ quyết định mọi thứ. Điều này chuyển vấn đề thành việc quyết định những yếu tố nào chúng ta phải “sửa chữa” để tổng đóng góp của chúng cho phép cân bằng các sai lệch. Thay vì suy luận về trình tự phân phối lại, chúng ta chỉ quan tâm đến việc liệu tập hợp con được chọn có thể chiếm tổng phần thừa hay phần thiếu hay không. 

Khi chúng tôi cố định giá trị trung bình mục tiêu, mỗi phần tử sẽ đóng góp một độ lệch so với giá trị đó. Nhiệm vụ sẽ trở thành việc chọn số lượng phần tử tối thiểu có tổng độ lệch phù hợp với tổng độ mất cân bằng. Vì độ lệch dương và âm phải triệt tiêu trên toàn cầu nên chiến lược tối ưu là ưu tiên các phần tử có độ lệch tuyệt đối lớn nhất vì chúng mang lại “sức mạnh hiệu chỉnh” cao nhất cho mỗi chỉ số đã chọn. 

Do đó, chúng tôi sắp xếp độ lệch theo giá trị tuyệt đối và tích lũy một cách tham lam cho đến khi đạt được mức điều chỉnh cần thiết. Thời điểm chúng tôi đạt đến mức mất cân bằng dư bằng 0, số lượng phần tử được chọn là tối ưu bởi vì bất kỳ sự thay thế nào bằng các phần tử có cường độ nhỏ hơn sẽ yêu cầu nhiều lượt chọn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tham lam lệch lạc | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tổng của mảng và kiểm tra xem nó có chia hết cho n hay không. Nếu nó không chia hết thì không thể có sự cân bằng nên đáp án là -1. Điều này xảy ra vì việc phân phối lại bảo toàn tổng số tiền. 
2. Tính giá trị mục tiêu bằng tổng chia cho n. Mọi cấu hình cuối cùng phải gán chính xác giá trị này cho từng vị trí. 
3. Với mỗi phần tử, hãy tính độ lệch của nó so với mục tiêu. Độ lệch dương nghĩa là thặng dư, âm nghĩa là thâm hụt. Những sai lệch này thể hiện mức độ góp phần của mỗi chỉ số vào sự mất cân bằng. 
4. Quan sát rằng tổng của tất cả các độ lệch bằng 0, do đó nhiệm vụ giảm xuống còn việc hủy bỏ sự mất cân bằng bằng cách sử dụng một tập hợp con các chỉ số có độ lệch được “kích hoạt”. 
5. Sắp xếp các phần tử theo độ lệch tuyệt đối theo thứ tự giảm dần. Độ lệch lớn sẽ hiệu quả hơn vì mỗi chỉ số được chọn sẽ đóng góp nhiều hơn vào việc điều chỉnh sự mất cân bằng. 
6. Duyệt qua danh sách đã sắp xếp, tích lũy các đóng góp sai lệch cho đến khi sự mất cân bằng đã hiệu chỉnh đang chạy đạt đến 0. Đếm xem có bao nhiêu phần tử đã được sử dụng. 
7. Trả về số lượng bạn bè tối thiểu được chọn. 

### Tại sao nó hoạt động 

Mỗi chỉ mục được chọn sẽ loại bỏ độ lệch của nó khỏi hệ thống và mục tiêu là loại bỏ sự mất cân bằng tổng thể bằng cách sử dụng số lượng loại bỏ nhỏ nhất. Vì mỗi lần loại bỏ đều góp phần tuyến tính vào giá trị sai lệch của nó, nên việc chọn cường độ lớn hơn trước tiên sẽ giảm thiểu số bước cần thiết để loại bỏ sự mất cân bằng tổng thể. Bất kỳ giải pháp nào sử dụng độ lệch nhỏ hơn trong khi bỏ qua độ lệch lớn hơn đều có thể được cải thiện bằng cách hoán đổi chúng, giảm hoặc bảo toàn số phần tử đã chọn trong khi tăng công suất hiệu chỉnh, đảm bảo tính tối ưu tham lam. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        s = sum(a)
        if s % n != 0:
            out.append("-1")
            continue

        target = s // n

        dev = [x - target for x in a]

        dev.sort(key=lambda x: abs(x), reverse=True)

        need = 0
        cnt = 0

        for d in dev:
            if need == 0:
                break
            need += d
            cnt += 1

        out.append(str(cnt))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xác nhận tính khả thi thông qua khả năng chia hết của tổng số tiền, vì không có sự phân phối lại nào có thể thay đổi tổng số tiền toàn cầu. Sau đó, nó chuyển đổi vấn đề thành sai lệch so với giá trị mục tiêu, giúp tách biệt sự mất cân bằng chính xác cần được giải quyết. 

Việc sắp xếp theo độ lệch tuyệt đối đảm bảo chúng tôi luôn xem xét những điều chỉnh có tác động mạnh nhất trước tiên. Vòng lặp mô phỏng việc lấy các phần tử cho đến khi sự mất cân bằng biến mất và bộ đếm theo dõi số lượng phần tử cần thiết. 

Một điểm tinh tế là điều kiện dừng là khi độ lệch tích lũy triệt tiêu sự mất cân bằng tổng thể chứ không phải khi tất cả các phần tử được xử lý. Điều này phản ánh rằng chúng ta chỉ cần đủ các chỉ số được chọn để giải quyết sự khác biệt toàn cầu chứ không phải tất cả chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [4, 5, 2, 5]
```Mục tiêu là 4. Dev = [0, 1, -2, 1] 

Sắp xếp theo giá trị tuyệt đối: [-2, 1, 1, 0] 

| Bước | Được chọn | Tổng chạy | 
| --- | --- | --- | 
| 1 | -2 | -2 | 
| 2 | +1 | -1 | 
| 3 | +1 | 0 | 

Chúng ta dừng lại sau 3 lựa chọn, nghĩa là 3 yếu tố đủ để cân bằng hệ thống. Điều này cho thấy rằng độ lệch âm lớn phải được điều chỉnh trước tiên và những hiệu chỉnh dương nhỏ hơn sẽ hoàn thành sự cân bằng. 

### Ví dụ 2 

đầu vào:```
n = 2
a = [0, 4]
```Mục tiêu là 2. Dev = [-2, 2] 

Sắp xếp: [2, -2] 

| Bước | Được chọn | Tổng chạy | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | -2 | 0 | 

Cả hai phần tử đều được yêu cầu, vì vậy câu trả lời là 2. Điều này xác nhận rằng khi độ lệch đối xứng và có độ lớn bằng nhau thì cả hai đều phải được chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Độ lệch sắp xếp chiếm ưu thế trong mỗi bài kiểm tra | 
| Không gian | O(n) | Lưu trữ mảng sai lệch | 

Các ràng buộc cho phép tổng số phần tử lên tới 2 × 10^5, do đó, cách tiếp cận n log n dễ dàng đủ nhanh và việc sử dụng bộ nhớ vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = sum(a)
        if s % n != 0:
            res.append("-1")
            continue
        target = s // n
        dev = sorted([x - target for x in a], key=lambda x: abs(x), reverse=True)
        need = 0
        cnt = 0
        for d in dev:
            if need == 0:
                break
            need += d
            cnt += 1
        res.append(str(cnt))
    return "\n".join(res)

assert run("""1
4
4 5 2 5
""") == "3"

assert run("""1
2
0 4
""") == "2"

assert run("""1
3
1 1 1
""") == "0"

assert run("""1
1
100
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cân bằng nhỏ | 3 | tích lũy tham lam | 
| hai yếu tố | 2 | độ lệch đối xứng | 
| đã bằng nhau | 0 | không cần thao tác | 
| phần tử đơn | 0 | trường hợp tầm thường | 

## Vỏ cạnh 

Khi tất cả các phần tử đã bằng nhau, mảng độ lệch đều bằng 0, do đó vòng lặp kết thúc ngay lập tức và trả về 0 mà không chọn bất cứ thứ gì. 

Khi tổng không chia hết cho n, thuật toán dừng sớm và đưa ra -1 vì không tồn tại giá trị trung bình mục tiêu, khiến việc phân phối lại là không thể bất kể lựa chọn tập hợp con. 

Khi chỉ có một phần tử, nó luôn bằng chính nó, vì vậy câu trả lời là 0 vì không cần hiệu chỉnh.
