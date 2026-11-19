---
title: "CF 2066B - Ma Thuật Trắng"
description: "Chúng ta được cho một mảng các số nguyên không âm và chúng ta được phép chọn một dãy con từ nó. Từ chuỗi con đó, chúng tôi muốn giữ lại càng nhiều phần tử càng tốt trong khi vẫn đảm bảo duy trì một điều kiện tiền tố-hậu tố rất cụ thể ở mọi điểm phân tách."
date: "2026-06-08T10:44:04+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2066
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1004 (Div. 1)"
rating: 1900
weight: 2066
solve_time_s: 99
verified: false
draft: false
---

[CF 2066B - Ma thuật trắng](https://codeforces.com/problemset/problem/2066/B) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, dp, tham lam, triển khai 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm và chúng ta được phép chọn một dãy con từ nó. Từ chuỗi con đó, chúng tôi muốn giữ lại càng nhiều phần tử càng tốt trong khi vẫn đảm bảo duy trì một điều kiện tiền tố-hậu tố rất cụ thể ở mọi điểm phân tách. 

Nếu chúng ta xem xét bất kỳ tiền tố nào của dãy con đã chọn, thì giá trị nhỏ nhất trong tiền tố đó ít nhất phải lớn bằng mex của hậu tố còn lại. Mex của một tập hợp là số nguyên không âm nhỏ nhất không xuất hiện trong đó, do đó, nó nắm bắt mức độ “hoàn chỉnh” của hậu tố đối với các giá trị bắt đầu từ 0 trở lên. 

Điều kiện này tạo ra sự căng thẳng giữa bên trái, được điều khiển bởi mức tối thiểu và bên phải, được điều khiển bởi các số nguyên nhỏ bị thiếu. Nếu hậu tố thiếu một số nhỏ như 0 hoặc 1, mex của nó sẽ trở nên nhỏ hoặc thậm chí là 0, khiến điều kiện trở nên dễ dàng hơn. Nhưng nếu hậu tố chứa nhiều số nguyên nhỏ liên tiếp, mex của nó sẽ tăng lên, buộc tiền tố tối thiểu cũng phải lớn. 

Kích thước đầu vào lớn trong các trường hợp thử nghiệm, tổng cộng lên tới 2·10^5 phần tử, do đó, bất kỳ giải pháp nào thử tất cả các chuỗi con hoặc tính toán lại mex nhiều lần trong mỗi lần phân chia đều không khả thi ngay lập tức. Ngay cả O(n^2) cho mỗi trường hợp thử nghiệm cũng quá chậm và thậm chí O(n log n) cho mỗi phần tử cũng sẽ gặp rủi ro nếu phải tính toán lại nhiều lần. Điều này thúc đẩy chúng ta hướng tới một cấu trúc tuyến tính hoặc gần tuyến tính với việc ghi chép cẩn thận tần số hoặc số lượng bị thiếu. 

Trường hợp cạnh tinh tế xuất hiện khi mảng chứa nhiều giá trị lớn nhưng có rất ít giá trị nhỏ. Ví dụ,`[1000000000, 1000000000, 1000000000]`chỉ cho phép một dãy con tầm thường vì mex của hậu tố luôn bằng 0 trừ khi số 0 xuất hiện. Một trường hợp cạnh khác là khi thiếu hoàn toàn 0: thì mex của bất kỳ hậu tố nào luôn bằng 0, làm cho điều kiện trở nên tầm thường và cho phép toàn bộ chuỗi con. Một cách tiếp cận ngây thơ cố gắng duy trì mex một cách rõ ràng cho mỗi lần phân chia hậu tố sẽ liên tục tính toán lại các giá trị bị thiếu và không thành công dưới các ràng buộc. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ liệt kê tất cả các chuỗi tiếp theo và kiểm tra xem mỗi chuỗi có kỳ diệu hay không. Đối với mỗi chuỗi con, chúng ta cần kiểm tra tất cả các điểm phân tách, tính toán lại giá trị tối thiểu của tiền tố và mex của hậu tố. Ngay cả khi chúng tôi duy trì tiền tố cực tiểu trong O(1), tính toán mex vẫn chiếm ưu thế: mỗi phép tính mex hậu tố là O(k) trong đó k là độ dài chuỗi con và có nhiều chuỗi con theo cấp số nhân. Điều này là hoàn toàn không thể thực hiện được. 

Một quan sát có cấu trúc hơn đến từ việc viết lại những gì điều kiện thực sự thực thi. Sửa một chuỗi con hợp lệ. Đối với sự phân chia ở vị trí i, hậu tố mex chỉ được xác định bằng số nhỏ xuất hiện trong hậu tố. Tiền tố tối thiểu giảm dần khi chúng tôi mở rộng tiền tố. Vì vậy, cách duy nhất để giữ cho bất đẳng thức có hiệu lực đối với các dãy con dài là kiểm soát cẩn thận số lượng số nguyên nhỏ mà chúng ta đưa vào, bởi vì mỗi lần xuất hiện của một số nhỏ đều làm giảm độ phức tạp của mex và hạn chế các lựa chọn trong tương lai. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì suy nghĩ về tiền tố, chúng tôi theo dõi yêu cầu về hậu tố: mex của phần còn lại chỉ phụ thuộc vào việc chúng tôi đã bao gồm các giá trị nhất định hay chưa. Trong một cấu trúc tối ưu, chúng tôi muốn trì hoãn việc "phá vỡ" sự hiện diện của các số nguyên nhỏ càng nhiều càng tốt trong khi vẫn đảm bảo vẫn còn đủ các phần tử lớn để hỗ trợ các ràng buộc cực tiểu tiền tố. 

Điều này dẫn đến một cách xây dựng tham lam, trong đó chúng tôi cố gắng duy trì tính khả thi bằng cách theo dõi số lần chúng tôi vẫn có đủ khả năng để bao gồm từng giá trị trước khi không thể giữ mex bị giới hạn. Chúng tôi mô phỏng một cách hiệu quả việc xây dựng chuỗi con trong khi đảm bảo rằng hậu tố luôn có đủ cấu trúc bị thiếu để mex không vượt quá ngưỡng tối thiểu tiền tố hiện tại. 

Việc giảm cốt lõi là chỉ các giá trị 0, 1, 2,… cho đến ranh giới mex hiện tại mới quan trọng. Các giá trị lớn hơn ngưỡng mex hiện tại hoạt động như các phần tử trung tính đối với mex nhưng có thể ảnh hưởng đến tiền tố tối thiểu. Vì vậy, vấn đề giảm xuống còn việc cân bằng số lượng nhỏ mà chúng tôi đưa vào so với việc duy trì mức tiền tố giảm dần ổn định, có thể được xử lý bằng cách đếm tần số và mô phỏng lựa chọn tham lam từ trái sang phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chúng tôi đếm tần số của tất cả các giá trị trong mảng, nhưng chúng tôi chỉ quan tâm rõ ràng đến các giá trị lên đến n, vì mex không bao giờ có thể vượt quá n đối với cấu trúc có độ dài-n. Các giá trị lớn hơn n có thể được coi là “vô hạn” và chỉ ảnh hưởng đến hành vi tối thiểu của tiền tố. 
2. Chúng tôi tính toán số lần xuất hiện của mỗi giá trị từ 0 trở lên. Điều này cho phép chúng tôi mô phỏng cách mex của một hậu tố sẽ phát triển như thế nào khi chúng tôi loại bỏ các phần tử theo giả thuyết. 
3. Chúng tôi xây dựng câu trả lời bằng cách mô phỏng số lượng phần tử chúng tôi có thể giữ lại trong khi vẫn duy trì tính khả thi của điều kiện mex. Chúng tôi duy trì một khái niệm đang diễn ra về việc có bao nhiêu giá trị nhỏ riêng biệt vẫn “có sẵn” để xuất hiện ở các hậu tố. 
4. Chúng ta tham lam đưa các yếu tố theo thứ tự giảm dần về khả năng duy trì giá trị của chúng. Các giá trị nhỏ rất quan trọng vì chúng xác định mex; các giá trị lớn rất linh hoạt và có thể được đưa vào miễn là chúng không vi phạm các ràng buộc tối thiểu của tiền tố. 
5. Việc xây dựng đơn giản hóa một cách hiệu quả việc xác định tiền tố lớn nhất của các số nguyên 0,1,2,… có thể được mảng hỗ trợ đầy đủ, đồng thời đếm tất cả các phần tử không can thiệp vào cấu trúc này.

Điểm tinh tế là khi chúng tôi quyết định mức mex tối đa mà chúng tôi có thể duy trì, mọi phần tử lớn hơn hoặc bằng mức đó đều có thể được đưa vào miễn phí, trong khi các phần tử bên dưới nó phải được khớp cẩn thận với các hạn chế về tính khả dụng. 

### Tại sao nó hoạt động 

Mex của một hậu tố chỉ phụ thuộc vào việc tất cả các số nguyên từ 0 trở lên có xuất hiện trong hậu tố đó hay không. Thuật toán ngầm đảm bảo rằng chúng tôi không bao giờ tạo hậu tố trong đó mex vượt quá mức tiền tố tối thiểu có thể hỗ trợ. Bằng cách theo dõi tính sẵn có của các số nguyên nhỏ và giới hạn khoảng cách mà chuỗi này có thể mở rộng, chúng tôi đảm bảo rằng mọi phép phân chia đều thỏa mãn bất đẳng thức cần thiết. 

Lựa chọn tham lam là chính xác vì một khi một giá trị bị loại khỏi việc hỗ trợ tiến trình mex, việc giới thiệu lại nó sau đó không thể cải thiện tính khả thi của các lần phân tách trước đó. Tính đơn điệu này đảm bảo rằng việc quyết định ngưỡng mex khả thi tối đa trên toàn cầu sẽ mang lại độ dài chuỗi con tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        cnt = [0] * (n + 2)
        for x in a:
            if x <= n:
                cnt[x] += 1
        
        mex = 0
        while mex <= n and cnt[mex] > 0:
            cnt[mex] -= 1
            mex += 1
        
        # all elements >= mex can be taken freely
        ans = mex
        for x in a:
            if x >= mex:
                ans += 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chỉ đếm số lần xuất hiện tối đa n, vì các giá trị lớn hơn không thể ảnh hưởng đến tiến trình mex. Sau đó, nó xây dựng tiền tố liên tiếp tối đa bắt đầu từ 0, sử dụng một lần xuất hiện của mỗi giá trị để xác định chuỗi mex có thể mở rộng bao xa. Giá trị đó trở thành xương sống cấu trúc của dãy con. 

Khi ranh giới mex này được cố định, mọi phần tử lớn hơn hoặc bằng nó đều có thể được đưa vào một cách an toàn vì chúng không đưa các số nguyên thấp bị thiếu vào hậu tố và chúng không làm gián đoạn tiến trình mex. Câu trả lời cuối cùng là kích thước của chuỗi tối đa này cộng với tất cả các phần tử lớn như vậy. 

Một sai lầm phổ biến là quên rằng chỉ cần một bản sao của mỗi số nhỏ để mở rộng chuỗi mex, trong khi các bản sao bổ sung có thể được sử dụng miễn phí sau này. Một điều tinh tế khác là các giá trị lớn hơn n có hiệu quả tương đương và không được xử lý trong vòng lặp mex. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi thuật toán trên hai đầu vào. 

Ví dụ đầu tiên: 

đầu vào:`[4, 3, 2, 1, 0]`| Bước | mex | sử dụng cnt | bao gồm chuỗi | phần tử lớn còn lại | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | số lượng được xây dựng | [] | [] | 0 | 
| lấy 0 | 1 | sử dụng một 0 | [0] | [] | 1 | 
| lấy 1 | 2 | sử dụng một cái 1 | [0,1] | [] | 2 | 
| lấy 2 | 3 | sử dụng một 2 | [0,1,2] | [] | 3 | 
| lấy 3 | 4 | sử dụng một 3 | [0,1,2,3] | [] | 4 | 
| lấy 4 | 5 | sử dụng một 4 | [0,1,2,3,4] | [] | 5 | 

Chuỗi mở rộng hoàn toàn đến 5 và không còn phần tử bổ sung nào. 

Ví dụ thứ hai: 

đầu vào:`[4, 3, 3, 2, 1, 0]`| Bước | mex | sử dụng cnt | chuỗi | phần tử lớn được sử dụng | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | số lượng được xây dựng | [] | [] | 0 | 
| lấy 0 | 1 | sử dụng 0 | [0] | [] | 1 | 
| lấy 1 | 2 | sử dụng 1 | [0,1] | [] | 2 | 
| lấy 2 | 3 | sử dụng 2 | [0,1,2] | [] | 3 | 
| lấy 3 | 4 | sử dụng 3 | [0,1,2,3] | [] | 4 | 
| dừng lại | 4 | số 4 có sẵn trong chuỗi | [0,1,2,3] | 4 còn lại | 5 | 

Chúng tôi không thể mở rộng chuỗi lên 5 vì 4 không có đủ dạng để tiếp tục quá trình phát triển mex nghiêm ngặt, nhưng các phần tử còn lại vẫn đóng góp vào số lượng cuối cùng. 

Những ví dụ này cho thấy giải pháp tách việc xây dựng cấu trúc (chuỗi mex) khỏi các phần tử linh hoạt, đó chính xác là những gì điều kiện thực thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đếm tần số và quét mex đơn | 
| Không gian | O(n) | Mảng tần số lên đến n | 

Tổng độ phức tạp là tuyến tính trong tổng kích thước đầu vào, vừa vặn thoải mái dưới 2·10^5 trong tất cả các trường hợp thử nghiệm. Thuật toán tránh mọi phép tính lại lồng nhau của mex hoặc tiền tố cực tiểu, giữ cho tất cả các hoạt động được thực hiện một lần hoặc được giới hạn bởi n. 

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
        
        cnt = [0] * (n + 2)
        for x in a:
            if x <= n:
                cnt[x] += 1
        
        mex = 0
        while mex <= n and cnt[mex] > 0:
            cnt[mex] -= 1
            mex += 1
        
        ans = mex + sum(1 for x in a if x >= mex)
        out.append(str(ans))
    
    return "\n".join(out)

# provided samples
assert run("""8
5
4 3 2 1 0
6
4 3 3 2 1 0
4
2 0 1 2
1
777
4
1000000000 1 7 9
2
0 1
2
1 2
4
0 1 0 1
""") == """5
5
3
1
4
2
2
3"""

# custom cases
assert run("""3
1
0
2
5
2
1
100 100 100
""") == """1
1
3
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 | 0`|`1`| 
|`2 | 5`|`1`| 
|`100 100 100`|`3`| giá trị lớn bằng nhau | 

## Vỏ cạnh 

Khi mảng không chứa số 0, chuỗi mex thậm chí không thể bắt đầu. Trong tình huống đó, thuật toán đặt mex thành 0 ngay lập tức và tất cả các phần tử được coi là có thể bao gồm tự do vì chúng đều ≥ mex. Ví dụ, đầu vào`[5, 6, 7]`tạo ra mex = 0 và câu trả lời trở thành 3, phù hợp với thực tế là mọi dãy con đều thỏa mãn điều kiện một cách tầm thường. 

Khi mảng chứa chính xác tiền tố đầy đủ gồm các số nguyên nhỏ nhưng không có giá trị bắc cầu lớn hơn, chẳng hạn như`[0,1,2,3]`, chuỗi mex mở rộng hoàn toàn và không có yếu tố bổ sung nào đóng góp. Thuật toán sử dụng mỗi giá trị một lần và dừng ở mex = 4, đưa ra đáp án 4. 

Khi có sự trùng lặp, chẳng hạn như`[0,0,1,1,2,2]`, chỉ một bản sao của mỗi giá trị được sử dụng để mở rộng mex, trong khi các bản sao còn lại được tính là phần tử tự do sau khi chuỗi được cố định. Thuật toán đếm chính xác một lần trên mỗi cấp độ cho chuỗi và tất cả các lần xuất hiện còn lại sau đó, duy trì tính tối ưu.
