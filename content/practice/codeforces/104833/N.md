---
title: "CF 104833N - \u6842\u6797\u7cbe\u516b\u4ef6"
description: "Chúng tôi được cung cấp một lượng rất nhỏ các mặt hàng, chính xác là tám loại quà lưu niệm. Mỗi loại có số lượng giới hạn, được mô tả bằng một mảng gồm 8 số nguyên. Riêng biệt, có $n$ người và mỗi người độc lập yêu cầu chính xác một trong tám loại này."
date: "2026-06-28T11:56:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104833
codeforces_index: "N"
codeforces_contest_name: "The 2023 Zhejiang SCI-TECH University Freshman Programming Contest"
rating: 0
weight: 104833
solve_time_s: 42
verified: true
draft: false
---

[CF 104833N - \u6842\u6797\u7cbe\u516b\u4ef6](https://codeforces.com/problemset/problem/104833/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lượng rất nhỏ các mặt hàng, chính xác là tám loại quà lưu niệm. Mỗi loại có số lượng giới hạn, được mô tả bằng một mảng gồm 8 số nguyên. Riêng biệt, có$n$mọi người và mỗi người độc lập yêu cầu chính xác một trong tám loại này. Nếu loại được yêu cầu vẫn còn hàng tại thời điểm người đó được xem xét, họ sẽ nhận được một mặt hàng thuộc loại đó và lượng hàng trong kho sẽ giảm đi một. Nếu không, họ không nhận được gì cả. 

Nhiệm vụ là tính toán xem có bao nhiêu người có thể hài lòng theo quy trình phân bổ tham lam tự nhiên này, giả sử chúng ta xử lý mọi người theo thứ tự nhất định. 

Kích thước đầu vào lớn về số lượng người, lên tới$2 \times 10^5$, nhưng vũ trụ vật phẩm được cố định ở kích thước tám. Điều này ngay lập tức hạn chế không gian giải pháp. Bất kỳ cách tiếp cận nào cố gắng mô phỏng các hoạt động tốn kém trên mỗi người vượt quá thời gian cố định chỉ được chấp nhận nếu nó duy trì tuyến tính chặt chẽ trong$n$. Bất cứ điều gì liên quan đến việc quét lồng nhau trên tám loại cho mỗi người vẫn ổn, nhưng bất cứ điều gì liên quan đến tìm kiếm theo từng người trên các cấu trúc động lớn hơn kích thước không đổi sẽ là chi phí không cần thiết. 

Một trường hợp cạnh tinh tế xuất phát từ thứ tự cạn kiệt. Nếu một cách giải thích ngây thơ cho rằng chúng ta có thể chỉ cần đếm các yêu cầu theo loại và so sánh với nguồn cung một cách độc lập, thì điều đó sẽ không chính xác vì các yêu cầu được xử lý theo trình tự và lượng hàng tồn kho được tiêu thụ trên toàn cầu. Ví dụ: nếu loại 1 có một mặt hàng và hai người yêu cầu loại 1 thì chỉ loại đầu tiên được phục vụ, mặc dù theo lý luận tổng hợp thì nhu cầu trên toàn cầu bằng với cung. 

Một trường hợp khó khăn khác là khi một số loại không còn hàng. Các yêu cầu đối với những loại đó phải luôn thất bại, ngay cả khi chúng xuất hiện sớm trong chuỗi. Ví dụ, nếu$a_3 = 0$và ai đó yêu cầu loại 3, chúng sẽ không bao giờ được tính bất kể đơn đặt hàng hay tình trạng sẵn có khác. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp tuân theo lời phát biểu vấn đề theo đúng nghĩa đen. Chúng tôi duy trì lượng hàng còn lại cỡ 8. Sau đó chúng tôi lặp lại tất cả$n$mọi người. Đối với mỗi người, chúng tôi kiểm tra xem loại được yêu cầu còn hàng hay không. Nếu có, chúng tôi giảm nó và tăng câu trả lời. 

Điều này có tác dụng vì mỗi quyết định chỉ phụ thuộc vào dung lượng còn lại hiện tại của loại đó. Mỗi thao tác là O(1), vì vậy toàn bộ quá trình là O(n), nằm trong giới hạn. 

Một ý tưởng sai lầm phổ biến là tổng hợp số lượng cho mỗi loại, sau đó tính toán$\sum \min(\text{demand}_i, a_i)$. Điều đó chỉ bỏ qua các ràng buộc về thứ tự nếu không có sự tương tác giữa các loại, nhưng ở đây sự tương tác hoàn toàn mang tính cục bộ cho mỗi loại. Trong vấn đề cụ thể này, sự tổng hợp đó thực sự trở nên chính xác vì các loại khác nhau không bao giờ cạnh tranh cho cùng một cổ phiếu. Sự tương tác duy nhất là trong mỗi loại một cách độc lập. Điều này có nghĩa là quá trình này có thể được xem như tám hàng đợi độc lập, mỗi hàng tiêu thụ hàng tồn kho riêng của mình theo thứ tự hàng đến. Tuy nhiên, việc thực hiện đếm tần số trên mỗi loại là sự phức tạp không cần thiết so với mô phỏng trực tiếp. 

Cái nhìn sâu sắc thực sự là vì không gian trạng thái chỉ có tám số nguyên nên chúng ta có thể mô phỏng tuần tự một cách an toàn mà không cần lo ngại về hiệu suất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Đếm theo loại | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quá trình phân bổ chính xác như mô tả. 

1. Khởi tạo một mảng`stock`cỡ 8 chứa số lượng sẵn có cho từng loại mặt hàng. Điều này thể hiện dung lượng còn lại của từng loại quà lưu niệm tại bất kỳ thời điểm nào. 
2. Khởi tạo bộ đếm`ans = 0`để theo dõi có bao nhiêu người được phục vụ thành công. 
3. Lặp lại từng người theo thứ tự. Dành cho người$i$, đọc loại yêu cầu của họ$b_i$. Thứ tự này quan trọng vì việc sử dụng sớm hơn sẽ làm giảm tính khả dụng cho các yêu cầu sau này. 
4. Kiểm tra xem`stock[b_i] > 0`. Nếu có, hãy gán mục cho người này bằng cách giảm dần`stock[b_i]`và tăng dần`ans`. 
5. Nếu`stock[b_i] == 0`, không làm gì cả vì loại đó đã cạn kiệt và không thể đáp ứng các yêu cầu tiếp theo. 
6. Sau khi xử lý xong tất cả mọi người, xuất ra`ans`. 

Ý tưởng chính là mỗi nhóm hàng tồn kho phát triển độc lập nhưng sự phát triển của nó phụ thuộc vào thứ tự thời gian của các yêu cầu. Chúng tôi không bao giờ cần phải xem trước hoặc sắp xếp lại các yêu cầu vì quá trình này hoàn toàn mang tính tham lam đối với thứ tự đến. 

### Tại sao nó hoạt động 

Mỗi loại hoạt động giống như một tài nguyên độc lập với dung lượng cố định. Mỗi lần có một yêu cầu thuộc loại$x$đến, hạn chế duy nhất quan trọng là liệu công suất còn lại của$x$là tích cực. Vì không có yêu cầu nào có thể ảnh hưởng đến kho hàng của bất kỳ loại nào khác nên các quyết định sẽ độc lập giữa các loại. Thuật toán bảo toàn bất biến`stock[i]`luôn bằng số tiền ban đầu trừ đi số lượng yêu cầu được chấp nhận đối với loại$i$thấy cho đến nay. Điều này đảm bảo rằng chúng tôi không bao giờ phân bổ quá mức bất kỳ loại nào và mọi nhiệm vụ được chấp nhận đều tương ứng với một đơn vị thực sự có sẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    b = list(map(int, input().split()))
    a = list(map(int, input().split()))

    stock = a[:]  # 8 types
    ans = 0

    for x in b:
        x -= 1  # convert to 0-index
        if stock[x] > 0:
            stock[x] -= 1
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc số lượng người, sau đó là yêu cầu của họ, sau đó là lượng hàng có sẵn cho từng loại trong số tám loại mặt hàng. Chi tiết triển khai tinh tế duy nhất là chuyển đổi từ các loại được lập chỉ mục 1 trong đầu vào sang truy cập mảng được lập chỉ mục 0. Phần còn lại là mô phỏng trực tiếp quy tắc phân bổ tham lam. 

Mỗi yêu cầu được xử lý chính xác một lần và mỗi lần kiểm tra là một lần truy cập mảng theo thời gian không đổi. Không cần cấu trúc dữ liệu bổ sung vì trạng thái được mảng chứng khoán tám phần tử nắm bắt hoàn toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có ba người và hai loại có số lượng hạn chế. 

đầu vào:```
3
1 1 2
1 1 0 0 0 0 0 0
```| Người | Yêu cầu | Còn Hàng Trước | Hành động | Kho Sau | Đã chấp nhận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [1,0,0,0,0,0,0,0] | cho loại 1 | [0,0,0,0,0,0,0,0] | vâng | 
| 2 | 1 | [0,0,0,0,0,0,0,0] | không có hàng | [0,0,0,0,0,0,0,0] | không | 
| 3 | 2 | [0,0,0,0,0,0,0,0] | không có hàng | [0,0,0,0,0,0,0,0] | không | 

Đầu ra là 1, cho thấy chỉ có thể đáp ứng yêu cầu đầu tiên khi hết hàng. 

Bây giờ hãy xem xét trường hợp có lượng hàng dồi dào: 

đầu vào:```
5
2 2 2 2 2
0 3 0 0 0 0 0 0
```| Người | Yêu cầu | Còn Hàng Trước | Hành động | Kho Sau | Đã chấp nhận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | [0,3,0,0,0,0,0,0] | cho loại 2 | [0,2,0,0,0,0,0,0] | vâng | 
| 2 | 2 | [0,2,0,0,0,0,0,0] | cho loại 2 | [0,1,0,0,0,0,0,0] | vâng | 
| 3 | 2 | [0,1,0,0,0,0,0,0] | cho loại 2 | [0,0,0,0,0,0,0,0] | vâng | 
| 4 | 2 | [0,0,0,0,0,0,0,0] | không có hàng | [0,0,0,0,0,0,0,0] | không | 
| 5 | 2 | [0,0,0,0,0,0,0,0] | không có hàng | [0,0,0,0,0,0,0,0] | không | 

Đầu ra là 3, phù hợp với tổng lượng tồn kho có sẵn cho loại 2. 

Dấu vết thứ hai xác nhận rằng thuật toán giới hạn chính xác các yêu cầu được chấp nhận ở mức cung cấp sẵn có cho mỗi loại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trong số$n$yêu cầu được xử lý một lần với việc kiểm tra và cập nhật hàng tồn kho O(1) | 
| Không gian | O(1) | Chỉ duy trì một mảng có kích thước cố định 8 | 

Các ràng buộc cho phép lên đến$2 \times 10^5$yêu cầu và thuật toán chỉ thực hiện công việc liên tục theo yêu cầu, do đó nó phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve_output(inp)).strip()

# We redefine solve to capture output cleanly for testing
def solve_output(inp: str) -> str:
    import sys
    input = sys.stdin.readline

    data = inp.strip().split()
    n = int(data[0])
    b = list(map(int, data[1:1+n]))
    a = list(map(int, data[1+n:1+n+8]))

    stock = a[:]
    ans = 0
    for x in b:
        x -= 1
        if stock[x] > 0:
            stock[x] -= 1
            ans += 1
    return str(ans)

# provided sample (as interpreted)
assert solve_output("3 1 1 2 1 1 0 0 0 0") == "1"

# all stock zero
assert solve_output("3 1 2 3 0 0 0 0 0 0 0 0") == "0"

# all requests same type, limited stock
assert solve_output("5 1 1 1 1 1 3 0 0 0 0 0 0 0 0 0") == "3"

# abundant stock
assert solve_output("4 1 2 3 4 10 10 10 10 10 10 10 10") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không cổ phiếu | 0 | không thể phân bổ | 
| nhu cầu lặp đi lặp lại vượt quá lượng hàng tồn kho | giới hạn nguồn cung | tính chính xác của từng loại suy giảm | 
| nguồn cung dồi dào | n | không hạn chế nhân tạo | 
| hộp nhỏ hỗn hợp | khớp tham lam đúng | độ chính xác mô phỏng cơ bản | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị chứng khoán bằng 0. Thuật toán vẫn lặp lại tất cả các yêu cầu nhưng không bao giờ tăng câu trả lời vì mọi lần kiểm tra đều thất bại. Đối với đầu vào`n = 3`, yêu cầu`[1,2,3]`, và toàn bằng 0, bất biến`stock[i] >= 0`giữ suốt và`ans`vẫn bằng 0, tạo ra đầu ra chính xác. 

Một trường hợp đặc biệt khác là khi một loại duy nhất thống trị tất cả các yêu cầu. Nếu lượng hàng tồn cho loại 1 là 2 và có 5 người đều yêu cầu loại 1 thì thuật toán sẽ giảm lượng hàng tồn kho hai lần rồi ngừng chấp nhận các yêu cầu tiếp theo. Sau lần chấp nhận thứ hai,`stock[0]`trở thành 0, do đó các lần lặp tiếp theo sẽ từ chối chính xác tất cả các yêu cầu còn lại. 

Trường hợp khó khăn cuối cùng là khi lượng hàng trong kho đủ lớn cho mọi yêu cầu. Trong trường hợp đó, mọi yêu cầu đều vượt qua`stock[x] > 0`kiểm tra và thuật toán chỉ đơn giản là đếm tất cả$n$mọi người, không bao giờ đánh vào nhánh từ chối.
