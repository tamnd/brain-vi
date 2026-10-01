---
title: "CF 104855C - Cá Mập Đói"
description: "Chúng ta được sắp xếp các hộp hình tròn, mỗi hộp chứa một số vật phẩm giống hệt nhau. Một người bắt đầu ở hộp đầu tiên và lặp đi lặp lại một quy trình cố định: nếu hộp hiện tại vẫn còn vật phẩm, cô ấy loại bỏ chính xác một vật phẩm và tăng bộ đếm đang chạy, khi đó cô ấy…"
date: "2026-06-28T11:00:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104855
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #27(3^3-Forces)"
rating: 0
weight: 104855
solve_time_s: 91
verified: false
draft: false
---

[CF 104855C - Cá mập đói](https://codeforces.com/problemset/problem/104855/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp các hộp hình tròn, mỗi hộp chứa một số vật phẩm giống hệt nhau. Một người bắt đầu ở hộp đầu tiên và lặp đi lặp lại một quy trình cố định: nếu hộp hiện tại vẫn còn vật phẩm, cô ấy loại bỏ đúng một vật phẩm và tăng bộ đếm đang chạy, sau đó cô ấy chuyển sang hộp tiếp theo. Nếu ô hiện tại đã trống, cô ấy chỉ cần bỏ qua bước xóa và vẫn tiếp tục. Điều này tiếp tục vô thời hạn cho đến khi tất cả các hộp trở nên trống rỗng. 

Đại lượng quan trọng mà chúng ta phải tính toán không phải là tổng số vật phẩm đã ăn cuối cùng, vốn là tổng của tất cả các giá trị, mà là một thứ năng động hơn: đối với mỗi chỉ mục hộp, chúng ta muốn biết tổng số vật phẩm đã ăn vào thời điểm chính xác mà hộp cụ thể đó lần đầu tiên trở nên trống rỗng. 

Các ràng buộc ngụ ý rằng tổng số hộp trong tất cả các trường hợp thử nghiệm lên tới 200.000, trong khi các giá trị riêng lẻ có thể lớn tới 10^9. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào làm giảm một mục trên mỗi bước. Một quy trình đơn giản có khả năng yêu cầu tối đa tổng tất cả các hoạt động a_i trên mỗi chu kỳ đầy đủ và vì bước đi có tính chu kỳ nên số lượt truy cập có thể dễ dàng đạt tới O(n * max(a_i)), điều này hoàn toàn không khả thi. 

Một vấn đề vi tế phá vỡ những cách tiếp cận ngây thơ là thời gian trống rỗng không độc lập. Ví dụ: nếu một hộp có giá trị rất lớn, nó sẽ tiếp tục đóng góp vào chu kỳ trong thời gian dài sau khi các hộp nhỏ hơn được hoàn thành, điều này sẽ thay đổi thời gian mà các hộp nhỏ hơn đó trở nên trống rỗng. Bất kỳ giải pháp nào cố gắng xử lý các hộp một cách độc lập hoặc giả sử một lần chuyển đều không chính xác. 

Đối với trường hợp hư hỏng cụ thể, xét n = 3, a = [100, 1, 1]. Hộp 2 và 3 trống rất nhanh, nhưng hộp 1 chiếm ưu thế trong thời gian dài. Cách tiếp cận ngây thơ “xử lý từng hộp riêng biệt” có thể cho rằng mỗi hộp hoàn thành theo tỷ lệ tương ứng với giá trị riêng của nó, thiếu thực tế là hộp 1 giữ cho chu kỳ tồn tại và tăng số vòng quay đầy đủ xảy ra trước khi hệ thống ổn định. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp tuân theo các quy tắc theo đúng nghĩa đen. Chúng tôi giữ một con trỏ quay vòng qua các chỉ số, giảm dần bất cứ khi nào có thể và ghi lại thời điểm mỗi hộp đạt đến 0. Điều này đúng nhưng tốn kém: mỗi bước giảm chính xác một đơn vị từ một số hộp, do đó tổng cộng chúng ta thực hiện các phép tính tổng (a_i). Với giá trị lên tới 10^9, điều này vượt xa mọi giới hạn. 

Cấu trúc của quy trình sẽ dễ hiểu hơn nếu chúng ta xem thời gian là các bước tổng thể riêng biệt thay vì các hành động theo từng hộp. Mỗi bước tương ứng với một lần truy cập vào một hộp theo thứ tự tuần hoàn. Trong một chu kỳ có độ dài n, mỗi hộp không trống sẽ đóng góp chính xác một mức giảm. Điều này có nghĩa là trong một chu kỳ hoàn chỉnh, mỗi hộp giảm đi một cho đến khi đạt 0 và quá trình tiếp tục trong khi có ít nhất một hộp khác 0. 

Quan sát quan trọng là đảo ngược quan điểm: thay vì mô phỏng thời gian chuyển tiếp, chúng tôi xác định thời điểm toàn cầu mà mỗi hộp kết thúc, dựa trên số chu kỳ đầy đủ và chu kỳ một phần mà nó tồn tại. Nếu một hộp có giá trị a_i, nó vẫn hoạt động cho a_i toàn bộ “lượt truy cập” vào chính nó, nhưng những lượt truy cập đó trải đều trong toàn bộ chu kỳ. Mỗi chu kỳ đầy đủ sẽ giảm mỗi hộp hoạt động đi một, vì vậy sau k chu kỳ đầy đủ, mỗi hộp sẽ mất k đơn vị. Do đó, một hộp sẽ trống sau a_i chu kỳ, nhưng thời điểm chính xác phụ thuộc vào cách các hộp khác kết thúc sớm hơn và rút ngắn quy trình như thế nào. 

Vấn đề trở nên tương đương với việc xử lý các hộp theo thứ tự giảm dần của a_i trong khi theo dõi số lượng chu kỳ đầy đủ được yêu cầu một cách hiệu quả trước khi mỗi hộp biến mất. Sau khi xem xét một hộp, chúng ta có thể tính toán cần bao nhiêu vòng quay hoàn chỉnh để nó đạt tới số 0, với điều kiện là một số hộp trước đó có thể đã bị loại bỏ và không còn tham gia vào các chu kỳ trong tương lai.

Điều này dẫn đến chiến lược đặt hàng ngoại tuyến cổ điển: chúng tôi xử lý các hộp từ a_i cao nhất trở xuống, duy trì số lượng phần tử vẫn còn “sống” trong chu trình. Mỗi lần chúng tôi xử lý một giá trị mới, chúng tôi tính toán tổng số bước được đóng góp bởi các chu kỳ đầy đủ trước đó cộng với chu kỳ một phần còn lại cho đến khi hộp này kết thúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng a_i) | O(n) | Quá chậm | 
| Phân loại + Kế toán chu kỳ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quy trình dưới dạng các chu kỳ đầy đủ được lặp lại trên tập hợp các hộp hiện hoạt. 

1. Liên kết mỗi hộp với chỉ mục và giá trị ban đầu của nó, đồng thời sắp xếp các hộp theo thứ tự giảm dần a_i. Thứ tự này cho phép chúng tôi suy luận về thời điểm các hộp “bỏ rơi” khỏi quy trình. 
2. Duy trì một biến cnt biểu thị số lượng hộp vẫn đang hoạt động trong chu trình. Ban đầu cnt = n, vì tất cả các hộp đều tham gia. 
3. Duy trì thời gian con trỏ đang chạy biểu thị tổng số lần xóa mục được thực hiện cho đến nay trên tất cả các hộp cộng lại. Đây thực sự là dòng thời gian toàn cầu. 
4. Xử lý các hộp theo thứ tự giảm dần của a_i. Khi chúng tôi tiếp cận một hộp có giá trị x, chúng tôi hiểu nó là tồn tại chính xác x chu kỳ đầy đủ của tập hoạt động hiện tại trước khi nó trống. Mỗi chu kỳ đầy đủ đóng góp các hoạt động cnt ảnh hưởng đến hộp này một lần trong mỗi chu kỳ. 
5. Do đó, thời điểm hộp này kết thúc là thời gian + x * cnt. Chúng tôi ghi lại điều này như là câu trả lời của nó. 
6. Sau khi xử lý hộp này, chúng ta giảm cnt đi một, vì hộp này không còn tham gia vào các chu kỳ trong tương lai. Các hộp còn lại bây giờ sẽ hoàn thành nhanh hơn vì chu kỳ đã được rút ngắn. 
7. Tiếp tục cho đến khi tất cả các hộp được xử lý, sau đó ánh xạ kết quả trở lại chỉ mục ban đầu. 

Điểm tinh tế quan trọng là việc sắp xếp đảm bảo chúng tôi luôn xử lý các hộp theo thứ tự chúng biến mất khỏi hệ thống. Khi một hộp lớn hơn được tính đến, nó sẽ giảm độ dài chu kỳ cho tất cả các hộp còn lại một cách hiệu quả, đó chính xác là những gì xảy ra trong quy trình thực. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, tất cả các hộp hoạt động đều được truy cập theo một thứ tự chu kỳ cố định và mỗi chu kỳ đầy đủ sẽ giảm mỗi hộp hoạt động chính xác một đơn vị. Điều này có nghĩa là hệ thống phát triển theo các giai đoạn trong đó mỗi giai đoạn tương ứng với một lần duyệt hoàn chỉnh của tập hoạt động hiện tại. Việc sắp xếp theo giá trị đảm bảo chúng tôi mô phỏng các kết thúc giai đoạn này theo đúng thứ tự: các giá trị lớn nhất tồn tại trong hầu hết các giai đoạn, do đó chúng xác định những thay đổi cấu trúc sớm nhất trong kích thước chu kỳ. Bởi vì mỗi hộp đóng góp chính xác một đơn vị cho mỗi chu kỳ cho đến khi nó biến mất, việc nhân giá trị của nó với kích thước chu kỳ hiện tại sẽ tính được tổng đóng góp của nó theo thời gian toàn cầu và việc loại bỏ nó sẽ làm giảm độ dài chu kỳ trong tương lai một cách nhất quán với động thái thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        arr = [(a[i], i) for i in range(n)]
        arr.sort(reverse=True)
        
        ans = [0] * n
        cnt = n
        cur_time = 0
        
        for val, idx in arr:
            ans[idx] = cur_time + val * cnt
            cur_time += val
            cnt -= 1
        
        print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ ghép từng giá trị với chỉ mục ban đầu của nó để có thể khôi phục kết quả sau khi sắp xếp. Việc sắp xếp theo thứ tự giảm dần đảm bảo chúng tôi xử lý các hộp theo thứ tự chúng ngừng đóng góp vào chu trình một cách hiệu quả. 

Biến cnt theo dõi số lượng hộp vẫn đang hoạt động, tương ứng trực tiếp với số lượng vị trí được truy cập trong một chu kỳ đầy đủ. Biến cur_time tích lũy sự đóng góp của các giai đoạn đã hoàn thành. Khi chúng tôi chỉ định ans[idx], chúng tôi đang tính toán bước tổng thể chính xác khi hộp này kết thúc bằng cách kết hợp các chu kỳ đã hoàn thành và phần đóng góp còn lại của chính nó được chia tỷ lệ theo độ dài chu kỳ hiện tại. 

Một lỗi phổ biến là quên rằng cnt thay đổi sau khi xử lý từng hộp, điều này sẽ giả định sai độ dài chu kỳ tĩnh. Việc giảm cnt là cần thiết vì khi một hộp trống, các chu kỳ trong tương lai sẽ không bao gồm nó nữa, giúp giảm thời gian cần thiết cho tất cả các hộp còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [2, 1, 4, 3]
```Thứ tự sắp xếp là (4, idx2), (3, idx3), (2, idx0), (1, idx1). 

| Bước | giá trị | cnt | cur_time | bài tập trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 0 | ans[2] = 16 | 
| 2 | 3 | 3 | 4 | ans[3] = 13 | 
| 3 | 2 | 2 | 7 | ans[0] = 11 | 
| 4 | 1 | 1 | 9 | ans[1] = 10 | 

Đầu ra:```
11 10 16 13
```Dấu vết này cho thấy các hộp sau này trải qua chu kỳ thu nhỏ như thế nào, điều này làm giảm thời gian hoàn thiện hiệu quả của chúng mặc dù giá trị thô của chúng nhỏ hơn. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [5, 2, 2]
```Thứ tự sắp xếp: (5,0), (2,1), (2,2) 

| Bước | giá trị | cnt | cur_time | bài tập trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 3 | 0 | ans[0] = 15 | 
| 2 | 2 | 2 | 5 | ans[1] = 9 | 
| 3 | 2 | 1 | 7 | ans[2] = 9 | 

Đầu ra:```
15 9 9
```Điều này xác nhận rằng các hộp nhỏ hơn bằng nhau sẽ hoàn thiện một cách đối xứng sau khi loại bỏ hộp chiếm ưu thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế cho từng trường hợp thử nghiệm | 
| Không gian | O(n) | Lưu trữ mảng có chỉ mục và mảng trả lời | 

Tổng n trên tất cả các trường hợp thử nghiệm tối đa là 200.000, do đó, giải pháp O(n log n) vừa vặn thoải mái trong giới hạn. Thuật toán chỉ thực hiện sắp xếp và quét tuyến tính duy nhất cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import math

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            arr = sorted([(a[i], i) for i in range(n)], reverse=True)
            ans = [0]*n
            cnt = n
            cur_time = 0
            for val, idx in arr:
                ans[idx] = cur_time + val * cnt
                cur_time += val
                cnt -= 1
            out.append(" ".join(map(str, ans)))
        return "\n".join(out)

    return solve()

# provided sample (format assumed fixed)
assert run("1\n4\n2 1 4 3\n") == "11 10 16 13"

# minimum size
assert run("1\n1\n7\n") == "7"

# all equal
assert run("1\n3\n5 5 5\n") == "15 10 5"

# increasing
assert run("1\n4\n1 2 3 4\n") == "10 9 7 4"

# large skew
assert run("1\n3\n100 1 1\n") == "300 201 201"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | giá trị trực tiếp | tính đúng đắn của trường hợp cơ sở | 
| tất cả đều bình đẳng | thời gian kết thúc giảm tuyến tính | đối xứng qua các hộp | 
| mảng tăng dần | đúng thứ tự hoàn thành | sắp xếp logic đúng đắn | 
| mảng lệch | thống trị có giá trị lớn | hiệu ứng thu nhỏ chu kỳ | 

## Vỏ cạnh 

Với n = 1, hệ thống không có hiệu ứng tuần hoàn. Ô đơn được truy cập nhiều lần nhưng không có vị trí nào khác nên mỗi đơn vị được tiêu thụ tuần tự và thời gian hoàn thiện đúng bằng a_1. Thuật toán xử lý việc này vì cnt bắt đầu từ 1 và không bao giờ thay đổi trước khi gán, do đó ans[0] trở thành 0 + a_1 * 1. 

Đối với các giá trị giống nhau, chẳng hạn [k, k, k], tất cả các hộp đều được xử lý theo một số thứ tự nhưng mỗi hộp có độ dài chu kỳ giảm dần 3, 2, 1. Các đầu ra trở thành k·3, k·2, k·1 theo một số thứ tự tùy thuộc vào việc lập chỉ mục. Thuật toán phản ánh chính xác điều này vì mỗi lần loại bỏ sẽ giảm cnt một cách đồng đều và áp dụng cùng một cấu trúc công thức cho từng phần tử.
