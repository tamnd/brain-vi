---
problem: 1272A
contest_id: 1272
problem_index: A
name: "Three Friends"
contest_name: "Codeforces Round 605 (Div. 3)"
rating: 900
tags: ["brute force", "greedy", "math", "sortings"]
answer: passed_samples
verified: true
solve_time_s: 445
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d9442-10c4-83ec-931d-e497b4ec06e0
---

#CF 1272A - Ba Người Bạn 

**Xếp hạng:** 900 
**Tags:** bạo lực, tham lam, toán học, phân loại 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 7 phút 25 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d9442-10c4-83ec-931d-e497b4ec06e0 

--- 

##Giải pháp 

## Hiểu vấn đề 

Ba người bạn ngồi trên trục số ở vị trí$a$,$b$, Và$c$. Trong một nước đi, mỗi người bạn có thể dịch chuyển tối đa một đơn vị sang trái, một đơn vị sang phải hoặc giữ nguyên vị trí của mình. Sau đúng một lần di chuyển đồng thời, chúng tôi xem xét mức độ chênh lệch của chúng và đo mức độ chênh lệch đó bằng tổng khoảng cách theo cặp giữa các vị trí cuối cùng của chúng. 

Nhiệm vụ là chọn nước đi cho cả ba người sao cho tổng khoảng cách theo cặp này càng nhỏ càng tốt. 

Quan sát chính về đầu vào là mỗi trường hợp thử nghiệm độc lập và chỉ bao gồm ba số nguyên, do đó mỗi trường hợp có thể được giải trong thời gian không đổi. Với tối đa 1000 trường hợp thử nghiệm, mọi giải pháp vượt quá$O(q)$hoặc$O(q \log q)$là không cần thiết, nhưng ngay cả$O(q)$với hằng số nặng sẽ ổn. 

Hạn chế thực sự quan trọng không phải là quy mô mà là cấu trúc. Chỉ với ba điểm và mỗi điểm chỉ có một đơn vị chuyển động, hệ thống có cấu hình có thể tiếp cận rất hạn chế. Về nguyên tắc, điều đó làm cho sức mạnh vũ phu đối với tất cả các kết hợp di chuyển trở nên khả thi, nhưng chúng ta vẫn cần nhận ra hình dạng của mục tiêu. 

Ví dụ: trường hợp cạnh tinh tế xuất hiện khi tất cả các vị trí đều bằng nhau$a=b=c=5$. Sau đó, bất kỳ chuyển động nào giữ chúng lại với nhau đều mang lại chi phí bằng 0 và bất kỳ cách tiếp cận ngây thơ nào giả định thứ tự riêng biệt hoặc chỉ tính toán các dịch chuyển điểm cuối đều có thể thất bại nếu nó quên rằng việc hợp nhất đã là tối ưu. 

Một trường hợp cạnh khác là khi hai điểm trùng nhau và điểm thứ ba ở xa, chẳng hạn như$1, 1000000000, 1000000000$. Cách tiếp cận ngây thơ “hướng về nhau một cách tham lam” phải xử lý cẩn thận rằng mỗi người chỉ được phép một đơn vị; bạn không thể thu gọn hoàn toàn những khoảng trống lớn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi khả năng cho nước đi của mỗi người bạn. Mỗi người bạn có ba lựa chọn: di chuyển sang trái, ở lại hoặc di chuyển sang phải. Điều đó mang lại$3^3 = 27$cấu hình cuối cùng có thể có cho mỗi trường hợp thử nghiệm. Đối với mỗi cấu hình, chúng tôi tính tổng khoảng cách theo cặp và lấy mức tối thiểu. Điều này đã đủ nhanh vì$27 \cdot 1000$là không đáng kể. 

Điều này hoạt động vì không gian trạng thái cực kỳ nhỏ. Hàm chi phí cũng đơn giản để đánh giá trong thời gian không đổi. 

Tuy nhiên, cấu trúc của vật kính cho phép có một cái nhìn rõ ràng hơn. Tổng khoảng cách theo cặp giữa ba điểm chỉ phụ thuộc vào thứ tự sắp xếp của chúng. Nếu chúng ta sắp xếp các vị trí cuối cùng như$x \le y \le z$, thì biểu thức trở thành:$$|x-y| + |x-z| + |y-z| = (z-x) + (z-y) + (y-x) = 2(z-x)$$Vì vậy, việc giảm thiểu tổng khoảng cách theo cặp tương đương với việc giảm thiểu khoảng cách giữa vị trí cuối cùng nhỏ nhất và lớn nhất. 

Điều này làm giảm vấn đề trong việc kiểm soát mức độ chặt chẽ mà chúng ta có thể đóng gói tối thiểu và tối đa sau khi mỗi điểm di chuyển tối đa 1. Theo trực giác, chúng ta muốn kéo điểm nhỏ nhất lên trên và điểm lớn nhất xuống dưới, trong khi điểm giữa có thể được điều chỉnh để trợ giúp hoặc duy trì ở giữa. 

Tác dụng có ý nghĩa duy nhất của mỗi lần di chuyển là liệu một điểm sẽ dịch chuyển về phía trung tâm hay ra xa nó. Vì mỗi điểm có thể di chuyển nhiều nhất là 1 nên cấu hình tốt nhất cuối cùng đạt được bằng cách thu hẹp phạm vi$[\min, \max]$càng nhiều càng tốt. 

Do đó, chiến lược tối ưu tương đương với: 

cố gắng di chuyển giá trị nhỏ nhất lên 1, giá trị lớn nhất giảm 1 và để giá trị ở giữa linh hoạt, sau đó tính toán lại phạm vi kết quả trên tất cả các lựa chọn nhất quán. Điều này sụp đổ để kiểm tra một số lượng trường hợp không đổi, hoặc quan sát trực tiếp hơn rằng câu trả lời chỉ phụ thuộc vào việc liệu chúng ta có thể giảm phạm vi ban đầu hay không. 

Vì mỗi điểm cuối có thể di chuyển vào trong nhiều nhất là 1, nên phạm vi mới tốt nhất có thể là:$$\max(0, (max - min) - 2)$$và tổng khoảng cách theo cặp gấp đôi phạm vi này. 

Điều này mang lại sự trực tiếp$O(1)$công thức cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu (27 tiểu bang) |$O(q)$|$O(1)$| Đã chấp nhận | 
| Tối ưu (giảm phạm vi) |$O(q)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào lý luận được tối ưu hóa dựa trên việc nén phạm vi. 

1. Đối với mỗi test case, hãy đọc ba vị trí$a, b, c$. Chúng ta coi chúng như một tập hợp nhỏ các điểm trên một đường thẳng. 
2. Tính giá trị tối thiểu và tối đa trong ba giá trị. Sự phân tán của hệ thống hoàn toàn được xác định bởi hai giá trị này vì các giá trị trung gian không ảnh hưởng đến khoảng cách xa nhất sau khi sắp xếp. 
3. Tính phạm vi ban đầu$d = \max(a,b,c) - \min(a,b,c)$. Điều này thể hiện tổng mức chênh lệch hiện tại. 
4. Quan sát xem chuyển động ảnh hưởng đến thái cực như thế nào. Điểm nhỏ nhất có thể tăng tối đa 1 và điểm lớn nhất có thể giảm tối đa 1, do đó phạm vi có thể co lại tối đa 2. 
5. Thay thế$d$với$\max(0, d - 2)$. Điều này thu được khả năng nén tốt nhất có thể sau một lần di chuyển. 
6. Đầu ra$2 \cdot \max(0, d - 2)$, vì tổng khoảng cách theo cặp giữa ba điểm bằng hai lần phạm vi sau khi được sắp xếp. 

### Tại sao nó hoạt động 

Đối với ba điểm bất kỳ, việc sắp xếp chúng cho thấy mục tiêu luôn bằng hai lần khoảng cách giữa cực tiểu và cực đại. Bất kỳ điểm ở giữa nào cũng đóng góp một cách đối xứng và triệt tiêu tổng. 

Vì mỗi điểm cuối có thể di chuyển vào trong nhiều nhất một đơn vị, nên cải thiện tốt nhất có thể cho nhịp là tổng cộng chính xác là hai đơn vị. Không có sự sắp xếp lại điểm giữa nào có thể cải thiện hơn những gì đã đạt được bằng cách nén các điểm cực trị, bởi vì nó không thể trở thành điểm cực trị trừ khi nó nằm bên ngoài hoặc khớp với chúng. 

Điều này làm cho vấn đề được xác định đầy đủ bằng mức độ mà các điểm cuối khoảng có thể thu hẹp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        a, b, c = map(int, input().split())
        mn = min(a, b, c)
        mx = max(a, b, c)
        d = mx - mn
        ans = max(0, d - 2) * 2
        print(ans)

if __name__ == "__main__":
    solve()
```Mã chỉ tính toán mức tối thiểu và tối đa cho mỗi trường hợp thử nghiệm, tránh mọi nhu cầu xem xét hoán vị hoặc giá trị trung bình. Phép trừ 2 mô hình trực tiếp thực tế là cả hai thái cực đều có thể di chuyển vào trong một lần. 

Một cạm bẫy triển khai phổ biến là quên phép nhân với 2 ở cuối. Đại lượng được giảm thiểu không phải là phạm vi mà là tổng khoảng cách theo cặp, luôn gấp đôi phạm vi cho ba điểm được sắp xếp. 

Một vấn đề tế nhị khác là xử lý chính xác các phạm vi nhỏ. Khi$d \le 2$, tất cả các điểm có thể được hợp nhất vào một vị trí sau khi di chuyển, vì vậy câu trả lời phải bằng 0. các`max(0, d - 2)`thực thi điều này. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp$10, 20, 30$. 

| Bước | phút | tối đa | phạm vi d | phạm vi điều chỉnh | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 10 | 30 | 20 | - | - | 
| sau khi nén | - | - | - | 18 | 36 | 

Ở đây cả hai thái cực đều hướng vào trong một cách tối ưu, giảm mức chênh lệch từ 20 xuống 18. Câu trả lời cuối cùng là$2 \cdot 18 = 36$, phù hợp với đầu ra. 

Bây giờ hãy xem xét$3, 3, 4$. 

| Bước | phút | tối đa | phạm vi d | phạm vi điều chỉnh | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | 4 | 1 | - | - | 
| sau khi nén | - | - | - | 0 | 0 | 

Hai điểm gần nhất đã cho phép thu gọn hoàn toàn sau một lần di chuyển, vì điểm tối đa có thể di chuyển sang trái và khớp với các điểm khác. 

Những ví dụ này cho thấy phép biến đổi chỉ phụ thuộc vào các điểm ngoài cùng chứ không phụ thuộc vào sự sắp xếp bên trong. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q)$| Mỗi trường hợp thử nghiệm yêu cầu thời gian tính toán tối thiểu/tối đa không đổi | 
| Không gian |$O(1)$| Không có bộ nhớ bổ sung ngoài các biến đầu vào | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện một số phép tính số học cho mỗi trường hợp kiểm thử, thấp hơn nhiều so với bất kỳ vấn đề giới hạn thời gian nào ngay cả đối với$q = 1000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        a, b, c = map(int, input().split())
        mn = min(a, b, c)
        mx = max(a, b, c)
        d = mx - mn
        out.append(str(max(0, d - 2) * 2))
    return "\n".join(out)

# provided samples
assert run("8\n3 3 4\n10 20 30\n5 5 5\n2 4 3\n1 1000000000 1000000000\n1 1000000000 999999999\n3 2 5\n3 2 6\n") == \
"0\n36\n0\n0\n1999999994\n1999999994\n2\n4"

# all equal
assert run("1\n5 5 5\n") == "0"

# already tight cluster
assert run("1\n1 2 3\n") == "2"

# large symmetric case
assert run("1\n1 1000000000 500000000\n") == str(2 * (999999999 - 2))

# two equal far third
assert run("1\n7 7 100\n") == "180"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | ổn định khoảng cách bằng không | 
| 1 2 3 | 2 | phạm vi tối thiểu khác không | 
| đối xứng lớn | tính toán | độ đúng số học biên | 
| hai bằng xa thứ ba | 180 | nén chính xác các cực trị | 

## Vỏ cạnh 

cho$a=b=c$, thuật toán tính toán$d=0$, sau đó kẹp về 0, tạo ra đầu ra 0 chính xác. Tất cả các bước di chuyển chỉ có thể tăng khoảng cách, do đó không có nhịp cấu hình nào ở cùng nhau. 

Đối với các bộ ba gần bằng nhau như$1,2,3$, phạm vi là 2. Sau khi trừ 2, nó trở thành 0, nghĩa là có thể thu gọn hoàn toàn bằng cách di chuyển các điểm cuối vào trong. Thuật toán nắm bắt điều này trực tiếp thông qua bước kẹp. 

Đối với sự tách biệt cực độ như$1, 1000000000, 999999999$, phạm vi lớn nhưng vẫn chỉ giảm đi 2. Thuật toán giải thích chính xác thực tế là chỉ có một đơn vị chuyển động cho mỗi điểm cuối, do đó mức giảm cuối cùng bị giới hạn nghiêm ngặt.
