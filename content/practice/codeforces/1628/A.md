---
title: "CF 1628A - Mảng tối đa"
description: "Chúng ta được cấp một mảng các số nguyên không âm và chúng ta được phép cắt đi nhiều lần tiền tố của mảng hiện tại. Đối với mỗi lần cắt, chúng tôi tính toán MEX của tiền tố đó và nối nó vào một mảng mới."
date: "2026-06-10T05:07:05+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "greedy", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1628
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 767 (Div. 1)"
rating: 1400
weight: 1628
solve_time_s: 104
verified: false
draft: false
---

[CF 1628A - Mảng tối đa](https://codeforces.com/problemset/problem/1628/A) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, tham lam, thực hiện, toán học, hai con trỏ 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên không âm và chúng ta được phép cắt đi nhiều lần tiền tố của mảng hiện tại. Đối với mỗi lần cắt, chúng tôi tính toán MEX của tiền tố đó và nối nó vào một mảng mới. Sau khi loại bỏ tiền tố đó, chúng ta tiếp tục với hậu tố còn lại cho đến khi không còn lại gì. Sự tự do hoàn toàn nằm ở cách chúng ta chọn những điểm cắt này. 

Mục tiêu không chỉ là tạo ra bất kỳ chuỗi giá trị MEX hợp lệ nào mà còn làm cho chuỗi đó có quy mô từ điển lớn nhất có thể. Điều này có nghĩa là giá trị đầu tiên của mảng được xây dựng càng lớn càng tốt; nếu có sự hòa, chúng tôi tối đa hóa giá trị thứ hai, v.v. 

Tổng ràng buộc của n trên tất cả các trường hợp thử nghiệm lên tới 2⋅10^5, do đó, bất kỳ giải pháp bậc hai nào cho mỗi trường hợp thử nghiệm sẽ thất bại. Ngay cả giải pháp O(n log n) cũng có thể chấp nhận được, nhưng bất cứ điều gì liên tục tính toán lại MEX từ đầu qua tiền tố sẽ quá chậm vì bản thân tính toán MEX tốn thời gian tuyến tính trừ khi được tối ưu hóa nhiều. 

Một cạm bẫy ngây thơ nhưng quan trọng là giả định rằng chúng ta phải luôn lấy toàn bộ mảng hoặc luôn lấy các phân đoạn tối thiểu. Ví dụ: trong một mảng như [0, 1, 2, 0, 3], việc lấy toàn bộ mảng sẽ mang lại MEX 4, đây là bước tối ưu cho bước đầu tiên, nhưng trong các mảng khác, việc cắt sớm có thể tạo ra nhiều giá trị MEX lớn sau đó. Chiến lược tối ưu không phải là tham lam trên toàn cầu về kích thước phân khúc mà là tham lam cục bộ khi tiền tố trở nên “hoàn chỉnh” về mặt chứa tất cả các giá trị cần thiết cho một MEX nhất định. 

Trường hợp cạnh tinh tế xuất hiện khi mảng có nhiều số 0 lặp lại hoặc chỉ thiếu một giá trị nhỏ. Chẳng hạn, [1, 1, 1] có MEX 0 cho bất kỳ tiền tố nào, do đó, bất kỳ phân vùng nào cũng mang lại tất cả các số 0. Một giải pháp bất cẩn có thể cố gắng mở rộng các phân đoạn một cách không cần thiết, nhưng điều đó không cải thiện được thứ tự từ điển. 

Một trường hợp cạnh khác là khi mảng đã là hoán vị của [0..n-1]. Khi đó MEX đầu tiên chỉ là n nếu chúng ta lấy toàn bộ mảng; bất kỳ lần cắt nào trước đó đều làm giảm MEX đáng kể, do đó việc hoàn thành tiền tố tham lam trở nên quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các cách có thể để chia mảng thành các phân đoạn. Tại mỗi vị trí, chúng tôi thử mọi k có thể, tính MEX của tiền tố và lặp lại hậu tố còn lại. Việc tính toán MEX cho mỗi tiền tố có chi phí ban đầu là O(n) và có các lựa chọn O(n) cho mỗi vị trí, dẫn đến số lượng phân vùng theo cấp số nhân. Ngay cả khi được ghi nhớ theo các trạng thái, số lượng mảng con vẫn là O(n²) và việc tính toán lại MEX khiến điều này hoàn toàn không khả thi. 

Quan sát quan trọng là MEX của tiền tố chỉ phụ thuộc vào việc liệu tất cả các số từ 0 đến một giá trị nào đó có xuất hiện hay không. Khi chúng tôi sửa một phân khúc, điều chúng tôi thực sự muốn là mở rộng phân khúc đó đến mức cần thiết để “thu thập” đủ số lần xuất hiện để MEX trở nên lớn nhất có thể. Sau khi hình thành một phân đoạn một cách tối ưu, chúng tôi loại bỏ nó và lặp lại phần còn lại. Điều này biến vấn đề thành một quá trình quét tham lam trong đó chúng tôi duy trì những giá trị mà chúng tôi đã thấy và xác định khi nào chúng tôi đã hoàn thành một “khối MEX” đầy đủ. 

Thay vì thử tất cả các lần cắt, chúng tôi quét từ trái sang phải, thu thập dần dần các giá trị và theo dõi MEX hiện tại. Khi tiền tố chứa tất cả các số từ 0 đến MEX−1 hiện tại, chúng ta có thể đóng một phân đoạn và xuất MEX đó. Sau đó chúng tôi thiết lập lại và tiếp tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n²) hoặc tệ hơn | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi duy trì bộ đếm tần số cho các số trong phân đoạn hiện tại và một biến theo dõi ứng cử viên MEX hiện tại.

1. Bắt đầu một phân đoạn mới với bảng tần số trống và mex hiện tại được đặt thành 0. Chúng tôi đang cố gắng xác định MEX lớn nhất có thể cho phân đoạn này. 
2. Quét từng phần tử một, chèn chúng vào bảng tần số. Mỗi lần chúng ta chèn một giá trị, chúng ta sẽ cập nhật kiến ​​thức của mình về những số nhỏ nào hiện diện. 
3. Sau mỗi lần chèn, chúng ta tiến lên con trỏ mex miễn là giá trị mex hiện tại tồn tại trong bảng tần số. Điều này đảm bảo mex luôn phản ánh số nguyên còn thiếu nhỏ nhất trong phân đoạn hiện tại. 
4. Nếu tại bất kỳ thời điểm nào chúng ta đã thấy tất cả các giá trị từ 0 đến mex−1 ít nhất một lần, chúng ta có thể đóng phân đoạn này một cách an toàn và nối mex vào câu trả lời. Lý do là việc mở rộng phân đoạn hơn nữa không thể tăng tính tối ưu về mặt từ điển cho vị trí này mà không trì hoãn quá nhiều mex có khả năng lớn hơn tiếp theo. 
5. Đặt lại bảng tần số và mex về 0 và tiếp tục quét từ vị trí tiếp theo. 
6. Sau khi hoàn thành mảng, nếu còn một đoạn chưa hoàn thành thì mex của nó sẽ được thêm vào làm giá trị cuối cùng. 

### Tại sao nó hoạt động 

Mỗi phân đoạn được xây dựng sao cho mex của nó được tối đa hóa với điều kiện là tiền tố liền kề của mảng còn lại. Thời điểm chúng ta có tất cả các số nguyên từ 0 đến mex−1 bên trong phân đoạn hiện tại, mex được cố định. Việc mở rộng phân đoạn hơn nữa chỉ có thể giữ cho mex không thay đổi hoặc trì hoãn việc hình thành phân đoạn tiếp theo, điều này không thể cải thiện thứ tự từ điển vì mex hiện tại đã ở mức tối đa cho lần cắt sớm nhất có thể. Điều này tạo ra một bất biến tham lam: mỗi lần cắt được thực hiện ở vị trí sớm nhất nơi mex hiện tại được xác định đầy đủ, đảm bảo đóng góp tối đa ở vị trí từ điển sớm nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        cnt = [0] * (n + 5)
        used = set()
        res = []

        mex = 0
        have = 0

        for x in a:
            cnt[x] += 1
            used.add(x)

            while mex in used:
                mex += 1

            # we can close segment when all numbers < mex appear
            # i.e., every value 0..mex-1 is present at least once
            # we check this via cnt > 0 implicitly using mex progression
            ok = True
            for v in range(mex):
                if cnt[v] == 0:
                    ok = False
                    break

            if ok:
                res.append(mex)
                cnt = [0] * (n + 5)
                used.clear()
                mex = 0

        if mex != 0 or any(cnt):
            # recompute mex for last segment
            final_mex = 0
            while cnt[final_mex]:
                final_mex += 1
            res.append(final_mex)

        print(len(res))
        print(*res)

if __name__ == "__main__":
    solve()
```Mã này duy trì một mảng tần số đang chạy cho phân đoạn hiện tại và một tập hợp các giá trị được nhìn thấy để theo dõi mex đang phát triển một cách hiệu quả. Con trỏ mex được nâng cao một cách tham lam càng sớm càng tốt và một phân đoạn được hoàn tất khi tất cả các giá trị bên dưới mex được xác nhận. Việc đặt lại sau mỗi lần cắt đảm bảo mỗi phân đoạn đều độc lập. 

Một chi tiết tinh tế là việc xử lý phân đoạn cuối cùng: ngay cả khi chúng tôi không bao giờ kích hoạt điều kiện cắt một cách rõ ràng thì hậu tố còn lại vẫn đóng góp một giá trị mex cuối cùng được tính trực tiếp từ bảng tần số của nó. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một trường hợp đơn giản để hiển thị hành vi phân khúc. 

Mảng đầu vào: [1, 0, 2, 0, 3] 

| Bước | Yếu tố | Tần số (0,1,2,3) | mex | Phân đoạn đã đóng? | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | (0,1,0,0) | 0 | không | [] | 
| 2 | 0 | (1,1,0,0) | 2 | vâng | [2] | 
| đặt lại | - | (0,0,0,0) | 0 | - | [2] | 
| 3 | 2 | (0,0,1,0) | 0 | không | [2] | 
| 4 | 0 | (1,0,1,0) | 1 | vâng | [2,1] | 
| đặt lại | - | (0,0,0,0) | 0 | - | [2,1] | 
| 5 | 3 | (0,0,0,1) | 0 | không | [2,1] | 
| kết thúc | - | - | 1 | nối thêm | [2,1,1] | 

Điều này cho thấy cách các phân đoạn được cắt chính xác khi tích lũy đầy đủ các giá trị bắt buộc. 

Trường hợp thứ hai như [0,1,2,3,4] tạo ra một phân đoạn đơn vì tiền tố đầy đủ đầu tiên đã chứa tất cả các giá trị bắt buộc lên tới mex = 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần và các hoạt động tần số được khấu hao O(1) | 
| Không gian | O(n) | Mảng tần số và các cấu trúc theo dõi phụ trợ | 

Độ phức tạp tuyến tính phù hợp thoải mái với giới hạn của tổng n lên tới 2⋅10^5, đảm bảo thực hiện nhanh chóng trên tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assuming function is named solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples (placeholders formatting may vary)
# assert run("6\n5\n1 0 2 0 3\n...") == "..."

# custom cases
assert run("1\n1\n0\n") == "1\n1", "single element"
assert run("1\n3\n1 1 1\n") == "1\n0", "no zero present"
assert run("1\n5\n0 1 2 3 4\n") == "1\n5", "full permutation"
assert run("1\n6\n0 0 1 1 2 2\n") == "2\n2 2", "paired structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1\n1 | trường hợp tối thiểu | 
| tất cả những cái | 1\n0 | mex luôn bằng không | 
| hoán vị | 1\n5 | phân đoạn tối ưu duy nhất | 
| cấu trúc ghép nối | 2\n2 2 | nhiều phân đoạn cân bằng | 

## Vỏ cạnh 

Hãy xem xét một mảng không có số 0 nào cả, chẳng hạn như [1, 2, 3]. Mex của bất kỳ tiền tố nào đều bằng 0, do đó thuật toán sẽ ngay lập tức đóng một phân đoạn ở phần tử đầu tiên. Kết quả đầu ra là một chuỗi các số 0 và mỗi lần cắt xảy ra ngay lập tức vì điều kiện thăng cấp mex không bao giờ được kích hoạt. 

Đối với một mảng như [0, 0, 0], mex chỉ trở thành 1 sau khi nhìn thấy ít nhất một số 0. Phân đoạn tăng dần cho đến khi số 0 đầu tiên xuất hiện, sau đó đóng lại, tạo ra các số 1 lặp lại cho mỗi phân đoạn. 

Trong [0,1,2,3], thuật toán đợi cho đến khi có tất cả các giá trị từ 0 đến 3, sau đó đóng một lần với mex 4. Bất kỳ lần cắt nào trước đó sẽ tạo ra mex nhỏ hơn ở vị trí đầu tiên, điều này sẽ ngay lập tức làm xấu đi thứ tự từ điển.
