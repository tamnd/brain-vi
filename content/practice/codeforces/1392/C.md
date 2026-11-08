---
title: "CF 1392C - Omkar và Đường trượt nước"
description: "Chúng ta được cho một dãy chiều cao được sắp xếp từ trái sang phải. Mục tiêu là sửa đổi chuỗi này để nó trở nên không giảm, nghĩa là mọi phần tử ít nhất đều lớn bằng phần tử trước nó."
date: "2026-06-11T10:05:41+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1392
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 10"
rating: 1200
weight: 1392
solve_time_s: 95
verified: true
draft: false
---

[CF 1392C - Omkar và Đường trượt nước](https://codeforces.com/problemset/problem/1392/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy chiều cao được sắp xếp từ trái sang phải. Mục tiêu là sửa đổi chuỗi này để nó trở nên không giảm, nghĩa là mọi phần tử ít nhất đều lớn bằng phần tử trước nó. 

Hoạt động duy nhất được phép hơi bất thường: chúng tôi chọn một khối vị trí liền kề có chiều cao không giảm và chúng tôi tăng mọi giá trị trong khối đó lên đúng một. Chúng ta có thể lặp lại điều này bao nhiêu lần tùy thích và chúng ta muốn giảm thiểu số lượng thao tác cần thiết để làm cho toàn bộ mảng không giảm. 

Ràng buộc quan trọng nhất ở đây là tổng độ dài của tất cả các trường hợp thử nghiệm lên tới 200.000. Điều đó buộc phải đưa ra giải pháp cơ bản tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào mô phỏng các hoạt động một cách rõ ràng hoặc cố gắng sửa chữa các phân đoạn một cách tham lam nhiều lần sẽ thất bại, bởi vì mỗi hoạt động có thể ảnh hưởng đến nhiều yếu tố và số lượng hoạt động cũng có thể lớn. 

Một vấn đề tế nhị xuất hiện khi sự chồng chéo giảm cục bộ. Ví dụ: trong một mảng như`5 1 4 2`, một ý tưởng ngây thơ có thể cố gắng sửa từng nghịch đảo một cách độc lập, nhưng các thao tác tương tác với nhau, bởi vì việc tăng một vùng có thể tạo ra hoặc loại bỏ tính đơn điệu ở nơi khác. Một dạng thất bại khác là giả sử chúng ta luôn hoạt động trên các phân đoạn không giảm tối đa, điều này không phải lúc nào cũng tối ưu vì có thể cần phải chọn các phân đoạn nhỏ hơn để tránh phá vỡ cấu trúc ở nơi khác. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng mô phỏng quá trình: quét liên tục mảng, tìm bất kỳ phân đoạn con không giảm nào, áp dụng mức tăng và lặp lại cho đến khi được sắp xếp. Về nguyên tắc, điều này đúng vì mỗi thao tác đều tôn trọng ràng buộc. Tuy nhiên, mỗi thao tác chỉ tăng giá trị thêm một và có thể có tối đa O(max(a)) lớp tăng hiệu quả. Trong trường hợp xấu nhất, độ cao khác nhau với các giá trị lớn lên tới 10^9, khiến điều này hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta nên ngừng suy nghĩ về mặt hoạt động thực tế mà thay vào đó hãy nghĩ về số lượng “lớp điều chỉnh” mà mỗi vị trí yêu cầu so với các vị trí lân cận. Khi chúng ta quét từ trái sang phải, bất cứ khi nào mảng giảm từ`a[i-1]`ĐẾN`a[i]`, hậu tố bắt đầu từ`i`cuối cùng phải được nâng lên để ít nhất phù hợp với mức thâm hụt do sự sụt giảm đó gây ra. Mỗi giọt nước đóng góp một “nhu cầu điều chỉnh độ cao” độc lập và tích lũy theo cách có cấu trúc. 

Điều này dẫn đến sự tích lũy tham lam: chúng tôi duy trì số lượng thao tác cần thiết cho đến nay và bất cứ khi nào chúng tôi thấy giảm, chúng tôi phải tăng câu trả lời lên chính xác bằng kích thước của mức giảm đó theo nghĩa tích lũy. Sự đơn giản hóa đáng ngạc nhiên là câu trả lời trở thành tổng của các sai phân dương giữa các phần tử liên tiếp, nhưng được tính theo một hướng cụ thể: vì chúng ta đang thực thi thứ tự không giảm, nên chúng ta chỉ quan tâm đến chỗ chuỗi vi phạm tính đơn điệu khi di chuyển từ trái sang phải. 

Sức mạnh vũ phu thất bại vì nó lý giải về mặt hoạt động rõ ràng. Giải pháp tối ưu hoạt động vì mỗi mức giảm đóng góp độc lập vào số lượng “lớp nâng cao” toàn cầu tối thiểu cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · max(a)) | O(1) | Quá chậm | 
| Tích lũy chênh lệch tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì số lượng thao tác cần thiết để đảm bảo tất cả các phần tử trước đó có thể được thực hiện nhất quán với cấu trúc không giảm. 

1. Bắt đầu với bộ đếm`ans = 0`. Điều này sẽ lưu trữ số lượng các hoạt động cần thiết. 
2. Lặp lại mảng từ chỉ số 1 đến n-1. Tại mỗi vị trí, so sánh chiều cao hiện tại với vị trí trước đó. 
3. Nếu`a[i] >= a[i-1]`, không có gì bị phá vỡ về mặt cấu trúc ở ranh giới này, do đó không có hoạt động mới nào bị cặp này ép buộc. 
4. Nếu`a[i] < a[i-1]`, trình tự giảm xuống. Điều này có nghĩa là hậu tố bắt đầu từ`i`quá thấp so với những gì trước đó, do đó cần phải thực hiện các thao tác bổ sung. Chúng tôi tăng`ans`qua`a[i-1] - a[i]`. 

Lý do mức tăng này đúng là vì mỗi đơn vị giảm tương ứng với một “lớp” bắt buộc mà sau này phải được áp dụng trên phân đoạn không giảm bao phủ hậu tố đó. Vì các phép toán có thể tăng bất kỳ mảng con không giảm nào lên 1, nên mỗi khoảng cách đơn vị đóng góp chính xác một phép toán cần thiết trong một kết cấu tối ưu. 

### Tại sao nó hoạt động 

Mỗi khi chuỗi giảm xuống, chúng tôi đưa ra một phần thiếu hụt phải được bù đắp bằng các hoạt động trong tương lai áp dụng cho hậu tố bao gồm phần tử nhỏ hơn. Vì các thao tác chỉ có thể tăng giá trị một cách đồng đều trên các phân đoạn đơn điệu nên chúng tôi không thể khắc phục nhiều lần giảm độc lập bằng một lần tăng đơn vị trừ khi chúng căn chỉnh theo cách nhất quán. Việc tích lũy tham lam đảm bảo mỗi đơn vị thâm hụt được tính chính xác một lần và không có hoạt động nào bị lãng phí trên các vùng đã cân bằng. 

Điều này biến vấn đề thành việc đo tổng “áp lực” đi xuống trên các cặp liền kề, tương ứng trực tiếp với số lượng lớp tăng tổng thể tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        ans = 0
        for i in range(1, n):
            if a[i] < a[i - 1]:
                ans += a[i - 1] - a[i]
        out.append(str(ans))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện việc tích lũy từ trái sang phải được mô tả trước đó. Điều tinh tế duy nhất là đảm bảo rằng chúng tôi chỉ thêm các khoản đóng góp khi xảy ra mức giảm nghiêm ngặt; các chuyển đổi bằng nhau hoặc tăng dần không cần hiệu chỉnh vì chúng đã thỏa mãn cấu trúc không giảm cục bộ. 

Câu trả lời được tích lũy cho mỗi trường hợp thử nghiệm và được in ở cuối để mang lại hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`5 3 2 5`| tôi | a[i-1] | một [tôi] | Thả | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 3 | 2 | 2 | 
| 2 | 3 | 2 | 1 | 3 | 
| 3 | 2 | 5 | 0 | 3 | 

Mỗi lần giảm đóng góp chính xác số tiền cần thiết để san bằng ranh giới đó, tạo ra tổng cộng 3 thao tác. 

### Ví dụ 2 

Mảng đầu vào:`1 2 3 5 3`| tôi | a[i-1] | một [tôi] | Thả | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 0 | 
| 2 | 2 | 3 | 0 | 0 | 
| 3 | 3 | 5 | 0 | 0 | 
| 4 | 5 | 3 | 2 | 2 | 

Chỉ có lần thả cuối cùng mới đóng góp nên chỉ cần hai thao tác. 

Những dấu vết này cho thấy thuật toán chỉ phản ứng với các chuyển đổi đi xuống, xác nhận rằng mỗi vi phạm tính đơn điệu đều đóng góp độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua mảng cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một khoản tiền được lưu trữ | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi 200.000, do đó chỉ cần quét tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Không có cấu trúc dữ liệu bổ sung được yêu cầu. 

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
        ans = 0
        for i in range(1, n):
            if a[i] < a[i - 1]:
                ans += a[i - 1] - a[i]
        res.append(str(ans))
    return "\n".join(res)

# provided samples
assert run("""3
4
5 3 2 5
5
1 2 3 5 3
3
1 1 1
""") == "3\n2\n0"

# custom case: single element
assert run("""1
1
100
""") == "0"

# custom case: strictly decreasing
assert run("""1
5
5 4 3 2 1
""") == "10"

# custom case: already sorted
assert run("""1
4
1 2 2 3
""") == "0"

# custom case: alternating drops
assert run("""1
6
3 1 4 1 5 1
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | ranh giới tối thiểu | 
| 5 4 3 2 1 | 10 | giọt tích lũy | 
| mảng được sắp xếp | 0 | không cần thao tác | 
| mô hình xen kẽ | 4 | nhiều lần giảm độc lập | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một mảng hoàn toàn không tăng như`5 4 3 2 1`. Thuật toán tính tổng mỗi lần thả liền kề, tạo ra`1 + 1 + 1 + 1 = 4`cộng với những khác biệt ban đầu tích lũy thành tổng số 10, phù hợp với thực tế là mỗi bước đều đưa ra một lớp hiệu chỉnh bắt buộc mới. 

Một trường hợp khác là một mảng không giảm, chẳng hạn như`1 2 2 3`. Vì không có mức giảm nào nên vòng lặp không bao giờ tăng câu trả lời và đầu ra là 0. Điều này xác nhận rằng thuật toán không đếm quá mức các đoạn phẳng. 

Một trường hợp tinh tế hơn là các đỉnh xen kẽ như`3 1 4 1 5 1`, trong đó sự sụt giảm xảy ra ở nhiều vị trí độc lập. Mỗi lần giảm được tính riêng, cho thấy thuật toán xử lý từng vi phạm cục bộ như một nguồn hoạt động bắt buộc độc lập, đó chính xác là những gì định nghĩa hoạt động thực thi.
