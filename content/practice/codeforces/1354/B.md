---
title: "CF 1354B - Chuỗi ba ngôi"
description: "Chúng ta được cung cấp nhiều ca kiểm thử và mỗi ca kiểm thử là một chuỗi chỉ gồm các ký tự 1, 2 và 3. Đối với mỗi chuỗi, chúng ta cần tìm đoạn liên tục ngắn nhất (chuỗi con) chứa ít nhất một lần xuất hiện của mỗi ký tự trong số ba ký tự."
date: "2026-06-11T13:54:17+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1354
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 87 (Rated for Div. 2)"
rating: 1200
weight: 1354
solve_time_s: 107
verified: true
draft: false
---

[CF 1354B - Chuỗi ba ngôi](https://codeforces.com/problemset/problem/1354/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, dp, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có nhiều ca kiểm thử và mỗi ca kiểm thử là một chuỗi chỉ gồm các ký tự`1`,`2`, Và`3`. Đối với mỗi chuỗi, chúng ta cần tìm đoạn liên tục ngắn nhất (chuỗi con) chứa ít nhất một lần xuất hiện của mỗi ký tự trong số ba ký tự. Nếu chuỗi thiếu hoàn toàn bất kỳ chữ số nào thì không thể tạo thành chuỗi con như vậy và câu trả lời là 0. 

Chuỗi con ở đây là một phần liền kề của chuỗi, vì vậy chúng ta được phép chọn chỉ mục bắt đầu và chỉ mục kết thúc và lấy mọi thứ ở giữa. Mục tiêu là giảm thiểu độ dài đó trong khi vẫn đảm bảo cả ba biểu tượng xuất hiện ít nhất một lần bên trong nó. 

Tổng cộng các ràng buộc rất lớn: tổng độ dài của tất cả các chuỗi trong các trường hợp thử nghiệm lên tới 200.000. Điều đó ngay lập tức loại trừ mọi giải pháp kiểm tra tất cả các chuỗi con một cách rõ ràng. Một cách tiếp cận mạnh mẽ thử mọi cặp điểm cuối sẽ yêu cầu O(n²) cho mỗi trường hợp kiểm thử trong trường hợp xấu nhất, tốc độ này quá chậm. 

Các trường hợp cạnh xuất hiện khi một hoặc nhiều chữ số bị thiếu hoàn toàn. Ví dụ, nếu`s = "111111"`, đầu ra đúng là`0`bởi vì không có`2`hoặc`3`. Tương tự, nếu một chuỗi chỉ chứa hai trong ba chữ số, chẳng hạn như`s = "112211"`, không có chuỗi con nào có thể thỏa mãn điều kiện. 

Một trường hợp khó phát hiện khác là khi chuỗi con hợp lệ không được hiển thị rõ ràng trong cấu trúc cục bộ. Ví dụ, trong`s = "1212123"`, phân khúc tối ưu có thể bao gồm một`3`chỉ xuất hiện một lần ở gần cuối, buộc cửa sổ phải kéo dài về phía sau. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ kiểm tra mọi chuỗi con có thể và kiểm tra xem nó có chứa`1`,`2`, Và`3`. Đối với mỗi chỉ số bắt đầu`i`, chúng tôi sẽ mở rộng chỉ số cuối`j`và theo dõi xem tất cả các ký tự được yêu cầu có mặt hay không. Ngay cả khi chúng tôi duy trì số lượng tần số tăng dần, chúng tôi vẫn kiểm tra các chuỗi con O(n²) trong trường hợp xấu nhất trên mỗi lần kiểm tra, dẫn đến khoảng 10¹⁰ thao tác trong trường hợp xấu nhất. Điều này không thể vượt qua theo các ràng buộc nhất định. 

Quan sát quan trọng là chúng ta không cần phải kiểm tra tất cả các chuỗi con một cách độc lập. Thay vào đó, chúng tôi chỉ quan tâm đến cửa sổ ngắn nhất bao gồm cả ba ký tự. Đây là một vấn đề cổ điển về cửa sổ trượt: khi chúng ta sửa ranh giới bên trái, chúng ta có thể di chuyển ranh giới bên phải cho đến khi tất cả các ký tự xuất hiện, sau đó cố gắng thu nhỏ cửa sổ từ bên trái trong khi vẫn duy trì tính hợp lệ. 

Cấu trúc của bài toán đảm bảo rằng việc mở rộng cửa sổ một cách đơn điệu sẽ làm tăng phạm vi bao phủ và việc thu nhỏ nó chỉ có thể làm giảm sự dư thừa mà không mất đi tính hợp lệ cho đến khi một ràng buộc bị vi phạm. Điều này làm cho kỹ thuật hai con trỏ là đủ. Chúng tôi duy trì số lượng tần số của`1`,`2`, Và`3`trong cửa sổ hiện tại và điều chỉnh con trỏ theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Cửa Sổ Trượt | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập bằng cách sử dụng cửa sổ trượt hai con trỏ. 

1. Khởi tạo hai con trỏ`l = 0`Và`r = 0`và một mảng tần số`cnt`có kích thước 4 để theo dõi sự xuất hiện của`1`,`2`, Và`3`. Chúng tôi cũng giữ một biến`best`được khởi tạo với số lượng lớn. 
2. Mở rộng con trỏ bên phải`r`từng bước bổ sung`s[r]`vào mảng tần số. Điều này dần dần xây dựng một cửa sổ ứng cử viên. 
3. Sau mỗi lần mở rộng, hãy kiểm tra xem cửa sổ hiện tại có chứa ít nhất một trong mỗi chữ số hay không. Điều kiện này tương đương với`cnt[1] > 0 and cnt[2] > 0 and cnt[3] > 0`. 
4. Sau khi thỏa mãn điều kiện, hãy thử thu nhỏ cửa sổ từ bên trái bằng cách di chuyển`l`chuyển tiếp trong khi vẫn giữ cả ba chữ số. Mỗi lần rút gọn hợp lệ đại diện cho một câu trả lời ứng viên tốt hơn (hoặc bằng nhau). 
5. Trong quá trình thu nhỏ này, hãy cập nhật câu trả lời với độ dài cửa sổ hiện tại`r - l + 1`bất cứ khi nào cửa sổ vẫn hợp lệ. 
6. Khi cửa sổ trở lại không hợp lệ sau khi thu nhỏ, hãy dừng thu nhỏ và tiếp tục mở rộng`r`. 
7. Nếu không tìm thấy cửa sổ hợp lệ nào, hãy quay lại`0`. 

Ý tưởng chính là mỗi khi chúng ta di chuyển`l`hoặc`r`, chúng ta đang thực hiện điều đó một cách đơn điệu trên toàn chuỗi, vì vậy mỗi chỉ mục được xử lý tối đa một số lần không đổi. 

Tại sao nó hoạt động được là do cấu trúc của các cửa sổ tối thiểu. Đối với bất kỳ điểm cuối bên phải cố định nào, chuỗi con kết thúc tốt nhất có thể có được bằng cách đẩy ranh giới bên trái sang bên phải nhiều nhất có thể trong khi vẫn giữ nguyên tất cả các ký tự được yêu cầu. Nếu chúng tôi không thu nhỏ tối đa, chúng tôi sẽ mang các ký tự không cần thiết và bất kỳ chuỗi con hợp lệ ngắn hơn nào kết thúc giống nhau`r`sẽ bị bỏ lỡ. Vì chúng ta đã thử tất cả`r`, chúng tôi bao gồm tất cả các ranh giới bên phải có thể có cho các chuỗi con tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        
        cnt = [0] * 4
        l = 0
        best = 10**9
        
        for r, ch in enumerate(s):
            x = ord(ch) - 48
            cnt[x] += 1
            
            while cnt[1] > 0 and cnt[2] > 0 and cnt[3] > 0:
                best = min(best, r - l + 1)
                cnt[ord(s[l]) - 48] -= 1
                l += 1
        
        if best == 10**9:
            print(0)
        else:
            print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một mảng tần số được lập chỉ mục trực tiếp theo giá trị chữ số, giúp tránh mọi chi phí ánh xạ. Con trỏ bên trái chỉ di chuyển về phía trước khi cửa sổ hợp lệ, đảm bảo tính chính xác trong khi vẫn giữ độ phức tạp tuyến tính. 

Một chi tiết tinh tế là bản cập nhật của`best`trước khi thu nhỏ lại. Điều này đảm bảo chúng tôi ghi lại cửa sổ hợp lệ nhỏ nhất cho mỗi điểm cuối bên phải trước khi có khả năng phá vỡ tính hợp lệ bằng cách di chuyển`l`. Một điểm quan trọng khác là khởi tạo`best`với giá trị trọng điểm, vì có thể không bao giờ tìm thấy chuỗi con hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét`s = "12222133333332"`. 

Chúng tôi theo dõi cách cửa sổ phát triển cho đến khi nó trở nên hợp lệ lần đầu tiên và sau đó thu nhỏ lại. 

| r | s[r] | tôi | cửa sổ | đếm (1,2,3) | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | (1,0,0) | mở rộng | 
| 1 | 2 | 0 | 12 | (1,1,0) | mở rộng | 
| 6 | 1 | 0 | 122221 | (2,4,0) | mở rộng | 
| 7 | 3 | 0 | 1222213 | (2,4,1) | hợp lệ, bắt đầu thu nhỏ | 
| 7 | 3 | 1 | 222213 | (1,4,1) | vẫn còn hiệu lực | 
| 7 | 3 | 2 | 22213 | (1,3,1) | vẫn còn hiệu lực | 
| 7 | 3 | 3 | 2213 | (1,2,1) | vẫn còn hiệu lực | 
| 7 | 3 | 4 | 213 | (1,1,1) | ghi lại câu trả lời | 

Điều này cho thấy việc thu nhỏ mạnh mẽ như thế nào để tìm thấy cửa sổ tối thiểu thực sự kết thúc tại`r = 7`. 

Bây giờ hãy xem xét`s = "31121"`. 

| r | s[r] | tôi | cửa sổ | đếm (1,2,3) | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | 3 | (0,0,1) | mở rộng | 
| 1 | 1 | 0 | 31 | (1,0,1) | mở rộng | 
| 2 | 1 | 0 | 311 | (2,0,1) | mở rộng | 
| 3 | 2 | 0 | 3112 | (2,1,1) | hợp lệ, thu nhỏ | 
| 3 | 1 | 1 | 112 | (2,1,0) | không hợp lệ | 

Tại thời điểm này, chúng tôi đã ghi lại độ dài 4 và các bản mở rộng sau này không tạo ra cửa sổ hợp lệ nhỏ hơn. Điều này xác nhận câu trả lời tối ưu được tìm thấy bằng cách giảm thiểu mỗi điểm cuối bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | mỗi con trỏ di chuyển nhiều nhất n lần | 
| Không gian | O(1) | mảng tần số có kích thước cố định | 

Tổng chiều dài trên tất cả các trường hợp thử nghiệm được giới hạn bởi 200.000, do đó chỉ cần quét tuyến tính trên tất cả các ký tự kết hợp là đủ. Thuật toán xử lý mỗi ký tự với số lần không đổi, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            s = input().strip()
            cnt = [0]*4
            l = 0
            best = 10**9
            for r, ch in enumerate(s):
                cnt[ord(ch)-48] += 1
                while cnt[1] and cnt[2] and cnt[3]:
                    best = min(best, r-l+1)
                    cnt[ord(s[l])-48] -= 1
                    l += 1
            out.append("0" if best == 10**9 else str(best))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""7
123
12222133333332
112233
332211
12121212
333333
31121
""") == """3
3
4
4
0
0
4"""

# all same character
assert run("""1
111111
""") == "0"

# missing one digit
assert run("""1
112211
""") == "0"

# already minimal
assert run("""1
123
""") == "3"

# scattered pattern
assert run("""1
321321
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chữ số giống nhau | 0 | thiếu ký tự bắt buộc | 
| chỉ có hai chữ số | 0 | trường hợp bất khả thi | 
| đã tối ưu | 3 | cửa sổ tối thiểu tồn tại ngay lập tức | 
| xen kẽ toàn bộ | 3 | cửa sổ trượt co đúng | 

## Vỏ cạnh 

Khi chuỗi thiếu hoàn toàn một trong các chữ số, điều kiện tần số không bao giờ được thỏa mãn, do đó thuật toán không bao giờ đi vào pha rút gọn và`best`vẫn không thay đổi. Vì`s = "333333"`, vòng lặp chỉ tính số gia cho`3`và không có cửa sổ hợp lệ nào được ghi lại, tạo ra kết quả`0`. 

Khi cửa sổ hợp lệ tồn tại nhưng lớn hơn nhiều so với cửa sổ tối thiểu, thuật toán sẽ dựa vào việc thu hẹp mạnh mẽ. TRONG`s = "12222133333332"`, một lần`3`được bao gồm, con trỏ trái sẽ tiến lên cho đến khi xuất hiện yêu cầu cuối cùng của`1`hoặc`2`sẽ bị mất, đảm bảo cửa sổ ở mức tối thiểu cho ranh giới bên phải đó. Điều này đảm bảo rằng không có tiền tố dư thừa nào được giữ lại và mọi chuỗi con tối ưu ứng cử viên đều được phát hiện trong quá trình quét.
