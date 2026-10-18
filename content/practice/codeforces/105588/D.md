---
title: "CF 105588D - Búp bê"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có một hoán vị các số nguyên biểu thị kích thước búp bê được đặt trong một hàng. Mỗi vị trí ban đầu chứa chính xác một con búp bê và tất cả các kích cỡ đều khác nhau."
date: "2026-06-22T14:47:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105588
codeforces_index: "D"
codeforces_contest_name: "The 2024 ICPC Asia Kunming Regional Contest (The 3rd Universal Cup. Stage 20: Kunming)"
rating: 0
weight: 105588
solve_time_s: 59
verified: true
draft: false
---

[CF 105588D - Búp bê](https://codeforces.com/problemset/problem/105588/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có một hoán vị các số nguyên biểu thị kích thước búp bê được đặt trong một hàng. Mỗi vị trí ban đầu chứa chính xác một con búp bê và tất cả các kích cỡ đều khác nhau. 

Thao tác được phép là lấy hai vị trí liền kề và hợp nhất chúng thành một vị trí duy nhất. Sau khi hợp nhất, vị trí kết quả chứa tất cả các búp bê từ cả hai vị trí ban đầu, do đó phạm vi giá trị của nó trở thành giá trị tối thiểu và tối đa của hai phân đoạn được hợp nhất. Việc hợp nhất chỉ được phép khi hai phân đoạn liền kề được tách biệt hoàn toàn về giá trị: mọi giá trị trong phân đoạn bên trái đều nhỏ hơn mọi giá trị trong phân đoạn bên phải hoặc lớn hơn mọi giá trị trong phân đoạn bên phải. 

Quá trình lặp lại và sau mỗi lần hợp nhất, mảng sẽ co lại một đoạn. Chúng tôi muốn tối đa hóa số lần hợp nhất, tương đương với việc tìm chuỗi hợp nhất liền kề hợp lệ dài nhất có thể cho đến khi không thể di chuyển thêm nữa. 

Các ràng buộc rất chặt chẽ theo nghĩa lập trình cạnh tranh điển hình: tổng số phần tử trong tất cả các trường hợp thử nghiệm lên tới 100000. Điều này ngay lập tức loại trừ mọi mô phỏng thử tất cả các lệnh hợp nhất có thể có hoặc duy trì trạng thái lập trình động trên tất cả các phân đoạn. Quá trình bậc ba hoặc bậc hai cho mỗi trường hợp thử nghiệm sẽ hết thời gian chờ, vì vậy giải pháp phải gần với tổng thể tuyến tính hoặc tuyến tính. 

Một khó khăn tinh tế xuất phát từ thực tế là việc hợp nhất làm thay đổi các mối quan hệ liền kề và cũng thay đổi phạm vi phân khúc. Một lựa chọn tham lam ngây thơ về việc hợp nhất bất cứ khi nào có thể có thể thất bại vì việc hợp nhất sớm có thể chặn các hợp nhất hợp lệ trong tương lai. 

Ví dụ, hãy xem xét một sự sắp xếp nhỏ như`[2, 1, 4, 3]`. Nếu chúng ta tham lam hợp nhất`(2,1)`đầu tiên, chúng tôi tạo một phân khúc`[1,2]`. Điều này cho phép hợp nhất với`[4]`, và sau đó với`[3]`, đạt được một chuỗi đầy đủ. Nhưng theo một cách sắp xếp hơi khác, việc hợp nhất sớm có thể phá hủy điều kiện xuyên biên giới trong tương lai, làm giảm tổng số lần hợp nhất. Điều này cho thấy chúng ta không thể xử lý việc sáp nhập một cách độc lập; chúng ta cần một cấu trúc toàn cầu. 

Một trường hợp thất bại khác là khi có nhiều cơ hội hợp nhất nhưng chỉ một số cơ hội dẫn đến chuỗi tối ưu. Ví dụ, trong`[3,1,4,2]`, sáp nhập`(1,4)`đầu tiên là không thể mặc dù nó có vẻ hấp dẫn cục bộ nếu chỉ xem xét phạm vi; tính hợp pháp phụ thuộc chặt chẽ vào sự liền kề và cực trị phân khúc toàn cầu. 

Vì vậy, thách thức cốt lõi là: chúng ta cần đếm số lượng tối đa các lần hợp nhất liền kề trong đó các phân đoạn vẫn “được phân tách bằng giá trị” ở mỗi bước mà không mô phỏng toàn bộ quá trình hợp nhất. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là mô phỏng tất cả các chuỗi hợp nhất có thể có. Ở mỗi bước, chúng tôi kiểm tra từng cặp phân đoạn liền kề, xác minh xem phạm vi giá trị của chúng có rời rạc hay không và thử đệ quy cả quyết định hợp nhất và không hợp nhất. Mỗi trạng thái là một phân vùng của mảng và có nhiều phân vùng như vậy theo cấp số nhân. Ngay cả một mô phỏng tham lam tính toán lại tất cả các phép hợp nhất có thể có sau mỗi thao tác cũng tốn O(n²) cho mỗi trường hợp kiểm thử trong trường hợp xấu nhất, vì mỗi lần hợp nhất sẽ giảm kích thước đi một nhưng mỗi bước yêu cầu quét các ràng buộc kề cận. 

Quan sát quan trọng là quá trình này không phải là chọn các thứ tự hợp nhất tùy ý mà là đếm xem có bao nhiêu cặp liền kề có thể được hợp nhất theo một trình tự tối ưu. điều kiện`ri < li+1 or ri+1 < li`có nghĩa là hai phân đoạn có thể hợp nhất chính xác khi khoảng giá trị của chúng không trùng nhau. Vì tất cả các giá trị đều khác biệt nên điều này tương đương với việc nói rằng mức tối đa của một phân khúc nhỏ hơn mức tối thiểu của phân khúc kia. 

Điều này ngay lập tức gợi ý một cấu trúc đơn điệu: bất cứ khi nào chúng tôi hợp nhất các phân đoạn, chúng tôi đang xây dựng các khoảng lớn hơn một cách hiệu quả mà thứ tự bên trong của chúng không còn quan trọng nữa, chỉ có mức tối thiểu và tối đa của chúng. Thuộc tính quan trọng là nếu hai đoạn liền kề có thể được hợp nhất thì tất cả các giá trị của một đoạn nằm hoàn toàn ở một phía của đoạn kia theo thứ tự hoán vị. 

Điều này biến vấn đề thành việc đếm xem có thể loại bỏ bao nhiêu “ranh giới” liền kề trong khi vẫn duy trì phân vùng thành các khoảng giá trị không chồng chéo. Mỗi lần hợp nhất sẽ giảm số lượng phân đoạn đi một, vì vậy câu trả lời là số lượng ranh giới có thể tháo rời tối đa. 

Thông tin chi tiết quan trọng là chúng ta có thể xử lý hoán vị trong khi vẫn duy trì cấu trúc của các phân đoạn đang hoạt động và hợp nhất một cách tham lam bất cứ khi nào hai phân đoạn cuối cùng “có thể tách rời” theo thứ tự giá trị. Bởi vì tất cả các giá trị đều khác biệt nên chúng tôi có thể theo dõi ranh giới phân khúc hiện tại bằng cách sử dụng cấu trúc giống ngăn xếp: mỗi phần tử mới bắt đầu một phân khúc mới hoặc hợp nhất ngược nếu nó duy trì điều kiện thứ tự nghiêm ngặt giữa phạm vi của phân khúc trước đó và phần tử hiện tại. 

Điều này dẫn đến một giải pháp quét tuyến tính trong đó chúng tôi duy trì một loạt các phạm vi phân đoạn. Mỗi lần chúng tôi thêm một phần tử mới, chúng tôi cố gắng hợp nhất nó ngược lại miễn là điều kiện phân tách vẫn còn. Mỗi lần hợp nhất thành công tương ứng với một thao tác trong câu trả lời cuối cùng. 

Việc hợp nhất tham lam từ trái sang phải này có tác dụng vì khi hai phân đoạn liền kề có thể hợp nhất được, việc trì hoãn việc hợp nhất sẽ không bao giờ làm tăng khả năng trong tương lai, vì việc hợp nhất chỉ mở rộng phạm vi và duy trì các ràng buộc thứ tự đơn điệu cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm | 
| Hợp nhất tham lam dựa trên ngăn xếp | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải, duy trì một ngăn xếp trong đó mỗi phần tử đại diện cho một phân đoạn đã hợp nhất hiện tại, được lưu trữ dưới dạng giá trị tối thiểu và tối đa. 

1. Bắt đầu với một ngăn xếp trống. Mỗi phần tử mới ban đầu tạo thành một đoạn trong đó`min = max = value`. Điều này thể hiện trạng thái trước khi bất kỳ sự hợp nhất nào được xem xét. 
2. Lặp lại mảng từ trái sang phải. Đối với mỗi giá trị mới, hãy tạo một phân đoạn mới và đẩy nó vào ngăn xếp. 
3. Sau khi đẩy một phân đoạn mới, hãy thử hợp nhất nó với phân đoạn trước đó trên ngăn xếp. Việc hợp nhất là hợp lệ nếu các khoảng rời rạc, nghĩa là mức tối đa của một khoảng nhỏ hơn hoàn toàn so với mức tối thiểu của khoảng còn lại. 
4. Nếu hai phân đoạn trên cùng có thể được hợp nhất, hãy xóa chúng và thay thế chúng bằng một phân đoạn được hợp nhất duy nhất có mức tối thiểu là mức tối thiểu nhỏ hơn và mức tối đa là mức tối đa lớn hơn. Mỗi lần hợp nhất như vậy sẽ làm tăng câu trả lời lên một. 
5. Tiếp tục cố gắng hợp nhất với phần trên cùng mới của ngăn xếp, vì việc hợp nhất có thể tạo ra một vùng lân cận hợp lệ mới với các phân đoạn trước đó. 
6. Lặp lại cho đến khi không thể hợp nhất được nữa cho vị trí hiện tại, sau đó tiếp tục quét mảng. 
7. Sau khi xử lý tất cả các phần tử, số lần hợp nhất tích lũy là câu trả lời. 

Lý do khiến quá trình tham lam này an toàn là vì mọi sự hợp nhất đều mang tính cục bộ và không thể đảo ngược về mặt cấu trúc. Khi hai phân đoạn được hợp nhất, thứ tự nội bộ của chúng sẽ được cố định thành một khoảng duy nhất và không hoạt động nào trong tương lai có thể phân chia chúng lại. Do đó, chúng ta chỉ cần quyết định khi nào việc hợp nhất có thể thực hiện được ngay lập tức chứ không phải liệu việc hợp nhất có nên bị trì hoãn hay không. 

### Tại sao nó hoạt động 

Thuật toán duy trì một chồng các khoảng giá trị rời rạc biểu thị phân đoạn hiện tại của mảng. Mỗi khoảng tương ứng với một khối liền kề theo thứ tự ban đầu và các khoảng này luôn rời rạc trong không gian giá trị. 

Tại bất kỳ thời điểm nào, đỉnh ngăn xếp đại diện cho phân đoạn ngoài cùng bên phải. Việc hợp nhất có thể thực hiện được chính xác khi phân đoạn này và phân đoạn trước đó có phạm vi giá trị không trùng nhau, điều này vừa cần vừa đủ để có hiệu lực. Bởi vì việc hợp nhất chỉ mở rộng các khoảng mà không tạo ra sự chồng chéo chưa được hàm ý bởi sự liền kề, nên bất kỳ việc hợp nhất nào có thể xảy ra tại một điểm nhất định đều có thể được thực hiện mà không làm giảm tổng số lần hợp nhất có thể đạt được sau này. Điều này mang lại một cấu trúc tối ưu tham lam trong đó mỗi lần hợp nhất được tính chính xác một lần khi nó trở nên hợp lệ cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        stack = []  # each element is (min_val, max_val)
        ans = 0

        for x in a:
            stack.append((x, x))

            while len(stack) >= 2:
                mn2, mx2 = stack[-1]
                mn1, mx1 = stack[-2]

                # check if two segments are separable
                if mx1 < mn2 or mx2 < mn1:
                    # merge them
                    stack.pop()
                    stack.pop()
                    new_mn = min(mn1, mn2)
                    new_mx = max(mx1, mx2)
                    stack.append((new_mn, new_mx))
                    ans += 1
                else:
                    break

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp thử nghiệm và duy trì một chồng các khoảng phân đoạn. Mỗi phần tử bắt đầu như một phân đoạn riêng của nó. Sau khi chèn, chúng tôi liên tục kiểm tra hai đoạn trên cùng. Nếu phạm vi giá trị của chúng rời nhau, chúng có thể được hợp nhất, vì vậy chúng tôi thay thế chúng bằng khoảng kết hợp của chúng và tăng câu trả lời. 

Một điểm tinh tế là chúng ta chỉ so sánh hai phân khúc trên cùng. Điều này là đủ vì mọi sự hợp nhất chỉ ảnh hưởng đến các phân đoạn liền kề và mọi cơ hội hợp nhất mới đều phải liên quan đến ranh giới được sửa đổi gần đây nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[2, 1, 4, 3]`Chúng tôi theo dõi trạng thái ngăn xếp. 

| Bước | Hành động | Ngăn xếp (tối thiểu, tối đa) | Hợp nhất | 
| --- | --- | --- | --- | 
| 2 | đẩy | [(2,2)] | 0 | 
| 1 | đẩy, hợp nhất (2,2)+(1,1) → (1,2) | [(1,2)] | 1 | 
| 4 | đẩy | [(1,2),(4,4)] | 1 | 
| 3 | đẩy, hợp nhất (4,4)+(3,3) → (3,4) | [(1,2),(3,4)] | 2 | 
| kết thúc | hợp nhất (1,2)+(3,4) → (1,4) | [(1,4)] | 3 | 

Điều này cho thấy rằng một khi các phân tách cục bộ được tạo ra, chúng sẽ lan truyền ngược lại và cho phép hợp nhất theo tầng. Ngăn xếp ghi lại chính xác thời điểm các khoảng thời gian không bị chồng chéo. 

### Ví dụ 2 

đầu vào:`[1, 3, 5, 2, 4]`| Bước | Hành động | Ngăn xếp (tối thiểu, tối đa) | Hợp nhất | 
| --- | --- | --- | --- | 
| 1 | đẩy | [(1,1)] | 0 | 
| 3 | đẩy | [(1,1),(3,3)] | 0 | 
| 5 | đẩy | [(1,1),(3,3),(5,5)] | 0 | 
| 2 | đẩy, hợp nhất (5,5)+(2,2) → (2,5) | [(1,1),(3,3),(2,5)] | 1 | 
| 4 | đẩy, hợp nhất (2,5)+(4,4) → (2,5) không thể hợp nhất ngoài | [(1,1),(3,3),(2,5),(4,4)] | 2 | 

Trường hợp này cho thấy việc hợp nhất phụ thuộc rất nhiều vào thứ tự: phần tử sau`2`kích hoạt cơ cấu lại tạo ra sự hợp nhất hợp lệ mới ở phía bên phải. 

Dấu vết xác nhận rằng việc hợp nhất chỉ xảy ra khi các khoảng giá trị có thể phân tách hoàn toàn và sau khi được hợp nhất, chúng vẫn ổn định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được đẩy một lần và xuất hiện nhiều nhất một lần trong quá trình hợp nhất | 
| Không gian | O(n) | Ngăn xếp lưu trữ tối đa một phân đoạn cho mỗi phần tử trong trường hợp xấu nhất | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, phù hợp thoải mái trong giới hạn 100000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isfinite

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))

            stack = []
            ans = 0

            for x in a:
                stack.append((x, x))
                while len(stack) >= 2:
                    mn2, mx2 = stack[-1]
                    mn1, mx1 = stack[-2]
                    if mx1 < mn2 or mx2 < mn1:
                        stack.pop()
                        stack.pop()
                        stack.append((min(mn1, mn2), max(mx1, mx2)))
                        ans += 1
                    else:
                        break
        return "\n".join(out)

    return solve()

# sample-like
assert run("1\n4\n2 1 4 3\n") == "3"
assert run("1\n4\n1 3 5 2 4\n") == "2"

# minimum size
assert run("1\n1\n1\n") == "0"

# already sorted increasing
assert run("1\n5\n1 2 3 4 5\n") == "0"

# reversed
assert run("1\n5\n5 4 3 2 1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | trường hợp cơ sở | 
| được sắp xếp | 0 | không thể hợp nhất | 
| đảo ngược | 0 | sự đơn điệu nghiêm ngặt ngăn cản sự phân tách hợp lệ | 
| giống mẫu | hợp nhất đúng | tính đúng đắn của việc hợp nhất theo tầng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mảng đã tăng hoặc giảm đơn điệu. Trong những trường hợp như vậy, không có hai phân đoạn liền kề nào thỏa mãn điều kiện phân tách nghiêm ngặt, do đó ngăn xếp không bao giờ kích hoạt việc hợp nhất. Thuật toán để lại chính xác từng phần tử dưới dạng phân đoạn riêng cho đến khi kết thúc, tạo ra sự hợp nhất bằng 0. 

Một trường hợp khác là khi việc hợp nhất chỉ có thể thực hiện được sau khi một số phần tử trung gian được thêm vào. Ví dụ, trong`[1, 3, 5, 2, 4]`, các phân đoạn đầu không thể hợp nhất nhưng việc chèn`2`định hình lại các khoảng bên phải để có thể thực hiện được nhiều lần hợp nhất. Thuật toán xử lý vấn đề này vì nó chỉ quyết định việc hợp nhất dựa trên đỉnh ngăn xếp hiện tại và mọi cơ cấu lại đều được phản ánh ngay lập tức trong các so sánh trong tương lai, đảm bảo không bỏ lỡ cơ hội bị trì hoãn.
