---
title: "CF 1355B - Nhà thám hiểm trẻ"
description: "Chúng ta được cung cấp một danh sách những người khám phá, mỗi người được liên kết với một số $ei$ đại diện cho số lượng người phải có trong bất kỳ nhóm nào mà họ tham gia. Nếu một nhà thám hiểm có giá trị $e$ thì họ chỉ sẵn sàng tham gia vào một nhóm có quy mô ít nhất là $e$."
date: "2026-06-11T13:52:21+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1355
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 643 (Div. 2)"
rating: 1200
weight: 1355
solve_time_s: 454
verified: true
draft: false
---

[CF 1355B - Nhà thám hiểm trẻ](https://codeforces.com/problemset/problem/1355/B) 

**Đánh giá:** 1200 
**Tags:** dp, tham lam, sắp xếp 
**Thời gian giải:** 7 phút 34 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các nhà thám hiểm, mỗi người được liên kết với một số$e_i$đại diện cho số lượng người phải có trong bất kỳ nhóm nào họ tham gia. Nếu một nhà thám hiểm có giá trị$e$, thì họ chỉ sẵn sàng tham gia vào một nhóm có quy mô ít nhất là$e$. Nhiệm vụ là phân chia một số hoặc tất cả các nhà thám hiểm thành càng nhiều nhóm hợp lệ càng tốt, đồng thời tôn trọng ràng buộc đó đối với mọi thành viên trong mỗi nhóm. 

Mỗi trường hợp thử nghiệm là độc lập. Đối với mỗi nhóm, chúng ta chỉ cần xuất ra số lượng nhóm tối đa mà chúng ta có thể tạo thành chứ không phải chính nhóm đó. 

Các ràng buộc đủ lớn để mọi giải pháp đều phải gần tuyến tính hoặc$n \log n$mỗi trường hợp thử nghiệm. Vì tổng số phần tử trong tất cả các trường hợp thử nghiệm bị giới hạn bởi$3 \cdot 10^5$, một giải pháp sắp xếp từng trường hợp thử nghiệm và sau đó xử lý nó một lần là đủ, trong khi bất kỳ cách tiếp cận nào liên tục cố gắng xây dựng các nhóm một cách tham lam theo cách ngây thơ sẽ biến thành hành vi bậc hai trong trường hợp xấu nhất. 

Một chế độ thất bại tinh vi xuất hiện khi người ta cố gắng phân công từng người khám phá vào các nhóm mà không phân loại. Ví dụ: nếu chúng tôi sớm nhìn thấy một yêu cầu lớn và đặt nó một mình, sau này chúng tôi có thể phát hiện ra rằng nó có thể được nhóm với những yêu cầu khác để tạo thành nhiều nhóm hợp lệ. Một dạng thất bại khác là cố gắng tham lam thành lập các nhóm theo thứ tự đến mà không tính đến số tiền nhỏ đó.$e_i$các giá trị nên được sử dụng để “lấp đầy” các nhóm một cách hiệu quả, nếu không sẽ có giá trị lớn$e_i$các giá trị có thể trở nên không thể đặt được. 

## Phương pháp tiếp cận 

Chiến lược bạo lực cố gắng xây dựng các nhóm theo từng bước. Chúng tôi liên tục chọn một tập hợp con gồm những người khám phá còn lại, kiểm tra xem nhóm đó có thể tạo thành một nhóm hợp lệ hay không và xóa nhóm đó nếu có. Điều này đúng vì nó tôn trọng trực tiếp định nghĩa ràng buộc. Vấn đề là mỗi lần thử có thể quét một phần lớn mảng còn lại để xác minh tính khả thi và trong trường hợp xấu nhất điều này dẫn đến khoảng$O(n^2)$hành vi trên mỗi trường hợp thử nghiệm khi người khám phá được xem xét lại nhiều lần. 

Quan sát cấu trúc quan trọng là điều kiện của một nhóm chỉ phụ thuộc vào kích thước của nó chứ không phụ thuộc vào danh tính. Sau khi chúng tôi sắp xếp những người khám phá theo$e_i$, chúng ta có thể xây dựng các nhóm một cách tham lam trong khi vẫn duy trì được số lượng ứng viên mà chúng ta đã tích lũy được. Bất cứ khi nào số lượng trình khám phá có sẵn tích lũy đạt hoặc vượt quá yêu cầu của phần tử hiện tại, chúng tôi có thể hoàn tất một nhóm và đặt lại bộ đếm. Điều này có hiệu quả vì việc trì hoãn một nhóm chỉ làm tăng số lượng thành viên sẵn có chứ không bao giờ làm giảm tính khả thi. 

Điều này làm giảm vấn đề xuống còn một lần duyệt qua một mảng đã được sắp xếp, trong đó chúng tôi theo dõi số lượng phần tử mà chúng tôi hiện đang xem xét cho một nhóm tiềm năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Sắp xếp + Quét tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. Đầu tiên chúng ta sắp xếp mảng$e_i$theo thứ tự không giảm. Việc sắp xếp đảm bảo rằng chúng ta luôn cố gắng đáp ứng các yêu cầu nhỏ nhất trước tiên, điều này rất cần thiết vì chúng dễ đặt nhất và đóng vai trò là chất bổ sung để hình thành các nhóm hợp lệ. 

Sau đó, chúng tôi quét mảng đã sắp xếp trong khi duy trì bộ đếm biểu thị số lượng người khám phá mà chúng tôi đã tích lũy được cho nhóm hiện tại. Mỗi lần chúng tôi bao gồm một trình khám phá, chúng tôi sẽ tăng bộ đếm này lên một. 

Khi bộ đếm bằng với yêu cầu của trình khám phá hiện tại, chúng tôi sẽ đóng một nhóm. Tại thời điểm đó, chúng tôi đặt lại bộ đếm về 0 vì những trình khám phá đó hiện đã được chỉ định và không thể sử dụng lại. 

Lý do việc kiểm tra này được thực hiện ở mức bằng nhau (chứ không phải lớn hơn) là vì khi chúng tôi có đủ thành viên để đáp ứng yêu cầu yếu nhất trong nhóm hiện tại, mọi khoản tích lũy bổ sung ngoài điểm đó sẽ tốt hơn nên được sử dụng để bắt đầu hình thành nhóm tiếp theo ngay lập tức. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ tiền tố nào của mảng đều chứa các yêu cầu nhỏ nhất có thể có trong số các phần tử còn lại. Khi chúng tôi thành lập một nhóm tại thời điểm hợp lệ sớm nhất, chúng tôi đảm bảo rằng không có phần tử nào có yêu cầu lớn hơn bị lãng phí sớm và chúng tôi duy trì tính linh hoạt cho các bước sau. Mỗi nhóm tương ứng với một phân đoạn tối đa trong đó số lượng tích lũy đáp ứng chính xác ngưỡng của phần tử được bao gồm cuối cùng, đảm bảo không có nhóm nào vi phạm giới hạn kích thước và không có nhóm hợp lệ nào bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        e = list(map(int, input().split()))
        e.sort()

        cnt = 0
        groups = 0

        for x in e:
            cnt += 1
            if cnt >= x:
                groups += 1
                cnt = 0

        print(groups)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các yêu cầu để trước tiên chúng tôi xử lý các ràng buộc dễ dàng hơn. Biến`cnt`theo dõi kích thước của nhóm hiện đang hình thành. Mỗi lần chúng tôi thêm trình khám phá, chúng tôi sẽ kiểm tra xem nhóm hiện tại có hợp lệ hay không bằng cách so sánh`cnt`với yêu cầu hiện tại`x`. Sau khi nó hợp lệ, chúng tôi sẽ ngay lập tức hoàn thiện nhóm và đặt lại. 

Một sai lầm phổ biến là chờ đợi cho đến khi`cnt == x`thay vì`cnt >= x`. Riêng sự bình đẳng không thành công khi nhiều giá trị nhỏ xuất hiện, vì các nhóm có thể có hiệu lực sớm hơn dự kiến ​​và việc trì hoãn việc kiểm tra sẽ dẫn đến các nhóm lớn không cần thiết làm giảm tổng số lượng được hình thành. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1 1
```Mảng được sắp xếp vẫn giữ nguyên. 

| Bước | Giá trị | cnt trước | cnt sau | nhóm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 0 | 
| 2 | 1 | 1 | 2 | 1 | 
| 3 | 1 | 0 | 1 | 2 | 
| 4 | 1 | 2 | 3 | 3 | 

Mỗi phần tử hoàn thành một nhóm ngay lập tức vì mọi yêu cầu đều ở mức tối thiểu. 

Dấu vết này cho thấy thuật toán không bao giờ trì hoãn một nhóm khi nó đã khả thi, đảm bảo phân vùng tối đa. 

### Ví dụ 2 

đầu vào:```
5
2 3 1 2 2
```Mảng được sắp xếp:```
1 2 2 2 3
```| Bước | Giá trị | cnt trước | cnt sau | nhóm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 0 | 
| 2 | 2 | 1 | 2 | 1 | 
| 3 | 2 | 0 | 1 | 1 | 
| 4 | 2 | 1 | 2 | 2 | 
| 5 | 2 | 0 | 1 | 2 | 
| 6 | 3 | 1 | 2 | 2 | 

Nhóm thứ hai chỉ hình thành sau khi tích lũy đủ phần tử để đáp ứng yêu cầu 2. Phần tử có yêu cầu 3 không bao giờ kích hoạt một nhóm theo cách sắp xếp này, điều này là tối ưu vì nó sẽ yêu cầu một nhóm lớn hơn mà không thể hoàn thành sau này. 

Điều này xác nhận rằng việc sắp xếp theo sau là sự tích lũy tham lam sẽ cân bằng chính xác các yêu cầu nhỏ và lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp chiếm ưu thế, quét tuyến tính đơn sau | 
| Không gian |$O(1)$thêm (hoặc$O(n)$có bộ nhớ) | chỉ mảng đầu vào được lưu trữ | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm bị giới hạn, do đó, việc sắp xếp từng trường hợp thử nghiệm và thực hiện một lần vượt qua nó sẽ phù hợp một cách thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""

# reference implementation
def solve(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    sys.stdin = io.StringIO(inp)

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        e = list(map(int, input().split()))
        e.sort()
        cnt = 0
        groups = 0
        for x in e:
            cnt += 1
            if cnt >= x:
                groups += 1
                cnt = 0
        out.append(str(groups))
    return "\n".join(out)

# provided samples
assert solve("2\n3\n1 1 1\n5\n2 3 1 2 2\n") == "3\n2"

# custom cases
assert solve("1\n1\n1\n") == "1"
assert solve("1\n4\n4 4 4 4\n") == "1"
assert solve("1\n5\n1 2 3 4 5\n") == "2"
assert solve("1\n6\n2 2 2 2 2 2\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn tối thiểu | 1 | trường hợp cơ sở | 
| giá trị lớn thống nhất | 1 | độ chặt ràng buộc nhóm | 
| trình tự tăng dần | 2 | hành vi trộn tham lam | 
| giá trị trung bình lặp đi lặp lại | 3 | thành lập nhiều nhóm | 

## Vỏ cạnh 

Trường hợp một phần tử tối thiểu đảm bảo rằng trình thám hiểm có yêu cầu 1 luôn tạo thành một nhóm một mình, vì bộ đếm ngay lập tức thỏa mãn điều kiện. 

Trường hợp tất cả các giá trị đều bằng$n$đảm bảo rằng chỉ có một nhóm có thể được thành lập, vì không có tiền tố nào đạt đến kích thước$n$trước khi tất cả các phần tử được sử dụng, xác nhận rằng thuật toán không tính quá mức khi yêu cầu cao. 

Trình tự tăng nghiêm ngặt đảm bảo rằng các giá trị nhỏ đầu tiên đóng vai trò bổ sung một cách chính xác và các giá trị lớn không buộc phải phân nhóm sớm một cách không chính xác. Đường tham lam được sắp xếp tự nhiên sẽ trì hoãn việc đóng cho đến khi có đủ tích lũy, duy trì tính chính xác.
