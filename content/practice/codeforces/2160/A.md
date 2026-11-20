---
title: "CF 2160A - Phân vùng MEX"
description: "Chúng ta được cấp một tập hợp các số nguyên và chúng ta được phép chia nó thành nhiều tập hợp nhỏ hơn sao cho mọi phần tử ban đầu được sử dụng chính xác với số lần tổng thể như nhau. Nói cách khác, chúng ta đang phân chia các lần xuất hiện của các số thành các nhóm."
date: "2026-06-08T00:05:02+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2160
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1058 (Div. 2)"
rating: 800
weight: 2160
solve_time_s: 177
verified: true
draft: false
---

[CF 2160A - Phân vùng MEX](https://codeforces.com/problemset/problem/2160/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên và chúng ta được phép chia nó thành nhiều tập hợp nhỏ hơn sao cho mọi phần tử ban đầu được sử dụng chính xác với số lần tổng thể như nhau. Nói cách khác, chúng ta đang phân chia các lần xuất hiện của các số thành các nhóm. 

Đối với mỗi nhóm, chúng ta có thể tính MEX của nó, số nguyên không âm nhỏ nhất còn thiếu trong nhóm đó. Một phân vùng được gọi là hợp lệ khi mọi nhóm đều có cùng giá trị MEX. Giá trị được chia sẻ đó trở thành điểm của phân vùng. Nhiệm vụ là chọn một phân vùng sao cho điểm này càng nhỏ càng tốt. 

Điều làm cho vấn đề này trở nên phức tạp là chúng ta không chọn một tập hợp và tính toán MEX của nó một cách trực tiếp. Chúng tôi đang phân phát các bản sao của các số thành nhiều nhóm và buộc tất cả các nhóm phải “thất bại” ở cùng một số còn thiếu đầu tiên. 

Các ràng buộc rất nhỏ: mỗi thử nghiệm có tối đa 100 phần tử và các giá trị cũng bị giới hạn bởi 100. Điều này ngay lập tức loại trừ mọi thứ theo cấp số nhân trên các phân phối hoặc phân vùng. Ngay cả lý luận bậc hai về tần số giá trị cũng an toàn, nhưng bất cứ điều gì liệt kê các phân vùng một cách rõ ràng đều không cần thiết và sẽ là quá mức cần thiết. 

Một sai lầm ngây thơ là cho rằng chúng ta chỉ nên tính MEX của toàn bộ mảng hoặc thứ gì đó gần với nó. Điều đó không thành công vì việc chia tách có thể làm giảm hoặc tăng số điểm có thể đạt được. Một sai lầm khác là nghĩ rằng chiến lược tốt nhất luôn là tối đa hóa quy mô nhóm, nhưng MEX phụ thuộc vào sự hiện diện của mọi số từ 0 trở lên chứ không phải tổng hoặc kích thước. 

Trường hợp cạnh cụ thể là khi chỉ tồn tại một số duy nhất. Ví dụ,`[0, 0, 0]`. Nếu chúng ta gộp mọi thứ vào một nhóm thì MEX là 1. Nhưng nếu chúng ta chia thành ba nhóm`{0},{0},{0}`, mỗi nhóm vẫn có MEX 1 nên điểm là 1. Một giải pháp bất cẩn có thể cho rằng việc chia tách sẽ làm thay đổi MEX một cách khó lường, nhưng ở đây nó vẫn giữ nguyên cấu trúc. 

Một trường hợp cạnh khác là khi thiếu hoàn toàn 0. Ví dụ`[1,2]`. Bất kỳ nhóm nào ngay lập tức có MEX 0, do đó câu trả lời buộc phải là 0 bất kể phân vùng. Điều này rất dễ bị bỏ sót nếu giả định MEX phụ thuộc vào giá trị cao hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách phân phối các lần xuất hiện thành nhiều nhóm và tính toán xem tất cả các nhóm có chia sẻ cùng một MEX hay không. Điều này liên quan đến việc phân vùng`n`các phần tử thành các tập hợp con tùy ý, tăng nhanh hơn theo cấp số nhân. Ngay cả đối với`n = 100`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là điểm số được xác định hoàn toàn bằng số lượng bản sao hoàn chỉnh của bộ`{0, 1, 2, ..., x-1}`chúng ta có thể hình thành giữa các nhóm. Nếu chúng ta muốn một nhóm có MEX`x`, thì mọi số từ`0`ĐẾN`x-1`phải xuất hiện trong nhóm đó ít nhất một lần, trong khi`x`phải thiếu. 

Để đạt được một phân vùng hợp lệ trong đó tất cả các nhóm có cùng MEX`x`, mỗi nhóm tiêu thụ ít nhất một lần xuất hiện của mỗi số`0..x-1`. Vì vậy, nếu chúng ta ký hiệu`freq[i]`là tần số của giá trị`i`, số lượng các nhóm như vậy mà chúng ta có thể hình thành bị giới hạn bởi tần số nhỏ nhất trong số`0..x-1`. 

Hãy để mức tối thiểu đó là`k`. Chúng ta có thể hình thành nhiều nhất`k`mỗi nhóm chứa một bản sao của mỗi số`0..x-1`. Tất cả các phần tử còn lại (bao gồm cả bản sao bổ sung) có thể được đặt ở bất cứ đâu mà không ảnh hưởng đến MEX miễn là chúng tôi đảm bảo`x`không xuất hiện trong bất kỳ nhóm nào. 

Để xây dựng hợp lệ, chúng ta cần ít nhất`k`các nhóm và chúng tôi cũng cần đảm bảo rằng`x`không ngẫu nhiên xuất hiện trong tất cả các nhóm. Hạn chế quan trọng là chúng ta phải đảm bảo rằng đối với một số`x`, cấu trúc khả thi và mang lại giá trị nhỏ nhất có thể. 

Điều này làm giảm việc kiểm tra đối với từng ứng cử viên MEX`x`, cho dù tất cả các số`0..x-1`tồn tại ít nhất một lần. Nếu thiếu bất kỳ phần nào, chúng ta không thể tạo thành một nhóm hợp lệ với MEX ≥ x. Như vậy nhỏ nhất`x`nơi điều này không thành công hoặc bị hạn chế sẽ xác định câu trả lời. 

Thay vì xây dựng các phân vùng, chúng tôi trực tiếp đưa ra lý do khi nào không thể duy trì MEX cao hơn trên tất cả các nhóm. Câu trả lời cuối cùng là chỉ số đầu tiên mà tần suất giảm xuống quá thấp để hỗ trợ phạm vi phủ sóng lặp lại giữa các nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các phân vùng | Hàm mũ | Hàm mũ | Quá chậm | 
| Quét dựa trên tần số | O (tối đa A) | O (tối đa A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mọi giá trị trong mảng. 

Điều này cho phép chúng tôi truy cập ngay vào số lượng bản sao của mỗi số mà chúng tôi có thể phân phối cho các nhóm. 
2. Bắt đầu kiểm tra các giá trị MEX dự kiến ​​từ 0 trở lên. 

Chúng tôi đang hỏi một cách hiệu quả: liệu chúng tôi có thể buộc tất cả các nhóm phải có ít nhất MEX không?`x`? 
3. Đối với cố định`x`, xác minh xem mọi số từ`0`ĐẾN`x-1`xuất hiện ít nhất một lần. 

Nếu thiếu bất kỳ phần tử nào, không nhóm nào có thể chứa tất cả các phần tử bắt buộc, vì vậy MEX không thể tiếp cận được`x`. 
4. Theo dõi lớn nhất`x`sao cho mọi số`0..x-1`hiện hữu. 

Đây là điểm mà tiền tố liên tiếp của số nguyên bị phá vỡ. 
5. Câu trả lời chính xác là độ dài tiền tố lớn nhất có thể đạt được này. 

Ngoài thời điểm này, MEX không thể tăng đều ở tất cả các nhóm. 

### Tại sao nó hoạt động 

Mỗi nhóm có cùng MEX buộc phải có một yêu cầu cấu trúc chung: mỗi nhóm phải chứa tất cả các số bên dưới MEX đó. Điều này làm giảm vấn đề kiểm tra xem tập hợp số “hoàn thành tiền tố” bao xa bắt đầu từ 0. Quyền tự do phân vùng không làm thay đổi yêu cầu này, vì thiếu một số trên toàn cầu có nghĩa là không thể thỏa mãn điều kiện trong bất kỳ nhóm nào. Do đó, câu trả lời chỉ phụ thuộc vào tiền tố liên tiếp dài nhất bắt đầu từ 0 tồn tại trong nhiều tập hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = [0] * 105
        for x in a:
            freq[x] += 1
        
        mex = 0
        while freq[mex] > 0:
            mex += 1
        
        print(mex)

if __name__ == "__main__":
    solve()
```Mảng tần số chỉ được sử dụng để phát hiện số nguyên bị thiếu đầu tiên bắt đầu từ 0. Vòng lặp tăng dần`mex`cho đến khi nó tìm thấy một giá trị có tần số bằng 0, đó chính xác là định nghĩa của MEX cho nhiều tập hợp. 

Chi tiết triển khai quan trọng là chúng tôi không hề cố gắng mô phỏng các phân vùng. Giải pháp hoàn toàn dựa vào quan sát rằng việc phân vùng không thay đổi liệu một giá trị có tồn tại trong nhiều tập hợp hay không và do đó không ảnh hưởng đến điều kiện tiền tố xác định tính khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0 0
```| mex | tần số[0] | quyết định | 
| --- | --- | --- | 
| 0 | 3 | tiếp tục | 
| 1 | 0 | dừng lại | 

Vòng lặp dừng ở số 1 vì số 1 bị thiếu hoàn toàn. Điều này cho thấy rằng mặc dù chúng ta có thể chia thành nhiều nhóm nhưng mỗi nhóm sẽ chỉ chứa các số 0, do đó MEX buộc phải bằng 1. 

### Ví dụ 2 

đầu vào:```
1 2
```| mex | tần số[mex] | quyết định | 
| --- | --- | --- | 
| 0 | 0 | dừng lại | 

Vì thiếu 0 nên không nhóm nào có thể chứa 0, nên mọi nhóm ngay lập tức có MEX 0. Thuật toán trả về chính xác 0. 

Những ví dụ này nhấn mạnh rằng câu trả lời chỉ phụ thuộc vào sự hiện diện của số nguyên còn thiếu nhỏ nhất chứ không phụ thuộc vào cách các phần tử được sắp xếp thành nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Chúng tôi quét mảng một lần để tạo tần số và sau đó quét phạm vi giới hạn lên tới 100 | 
| Không gian | O(1) | Kích thước mảng tần số cố định (≤ 101) | 

Các ràng buộc đủ nhỏ để ngay cả việc quét tuyến tính đơn giản cho mỗi trường hợp thử nghiệm cũng có thể dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        freq = [0]*105
        for x in a:
            freq[x] += 1
        mex = 0
        while freq[mex] > 0:
            mex += 1
        out.append(str(mex))
    return "\n".join(out)

# provided samples
assert run("""2
3
0 0 0
2
1 2
""") == """1
0"""

# custom cases
assert run("""1
1
0
""") == "1", "single zero"

assert run("""1
1
5
""") == "0", "missing zero"

assert run("""1
5
0 1 2 3 4
""") == "5", "full prefix"

assert run("""1
6
0 0 1 1 2 2
""") == "3", "balanced duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | 1 | MEX hợp lệ tối thiểu khi tồn tại 0 | 
| đơn 5 | 0 | thiếu 0 lực MEX 0 | 
| 0..4 hiện tại | 5 | tiền tố liên tiếp đầy đủ | 
| trùng lặp được ghép nối | 3 | trùng lặp không ảnh hưởng đến MEX | 

## Vỏ cạnh 

Đối với một đầu vào như`[0]`, kiểm tra tần số thấy`freq[0] > 0`và di chuyển đến`1`, Ở đâu`freq[1] = 0`. Đầu ra của thuật toán`1`, phù hợp với thực tế là bất kỳ phân vùng nào (chỉ có thể có một nhóm) đều có MEX 1. 

Đối với một đầu vào như`[2,3,4]`, vòng lặp ngay lập tức thất bại tại`mex = 0`, từ`0`vắng mặt. Điều này tạo ra đầu ra`0`và bất kỳ phân vùng nào cũng phải có MEX 0 vì không nhóm nào có thể chứa 0. 

cho`[0,1,1,2,2]`, vòng lặp tiến hành thông qua`0,1,2`nhưng dừng lại ở`3`. Điều này xác nhận rằng việc trùng lặp không thành vấn đề, chỉ khi mỗi số nguyên xuất hiện ít nhất một lần.
