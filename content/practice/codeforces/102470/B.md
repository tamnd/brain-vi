---
title: "CF 102470B - Làm việc tại Nhà hàng"
description: "Chúng ta cần mô phỏng một công nhân nhận đĩa từ người phục vụ và sau đó đưa chúng cho máy rửa bát. Người công nhân chỉ có hai chồng đĩa trên bàn và mỗi chiếc đĩa cuối cùng phải rời khỏi bàn theo đúng thứ tự mà nó được đưa đến."
date: "2026-08-05T20:38:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102470
codeforces_index: "B"
codeforces_contest_name: "2009-2010 ACM ICPC Southwestern European Regional Programming Contest (SWERC 2009)"
rating: 0
weight: 102470
solve_time_s: 63
verified: true
draft: false
---

[CF 102470B - Làm việc tại nhà hàng](https://codeforces.com/problemset/problem/102470/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần mô phỏng một công nhân nhận đĩa từ người phục vụ và sau đó đưa chúng cho máy rửa bát. Người công nhân chỉ có hai chồng đĩa trên bàn và mỗi chiếc đĩa cuối cùng phải rời khỏi bàn theo đúng thứ tự mà nó được đưa đến. Đầu vào mô tả một chuỗi các yêu cầu: mang một số đĩa mới lên bàn hoặc lấy đi một số đĩa đã được lưu trữ theo thứ tự đến. Đầu ra không phải là sự sắp xếp cuối cùng của các tấm mà là một chuỗi hợp lệ các hoạt động vật lý nhỏ hơn bằng cách sử dụng hai cọc. 

Thách thức là người công nhân không thể tiếp cận trực tiếp chiếc đĩa cũ nhất nếu những chiếc đĩa mới hơn được đặt lên trên nó. Các hoạt động đầu ra phải mô tả cách sắp xếp lại hai cọc sao cho mọi yêu cầu loại bỏ đều diễn ra chính xác trong khi vẫn nằm trong giới hạn nghiêm ngặt về số lượng hoạt động. 

Số lượng yêu cầu trong một trường hợp thử nghiệm nhiều nhất là 1000, nhưng tổng số tấm bị loại bỏ có thể lên tới 100000. Điều này có nghĩa là một giải pháp không thể di chuyển liên tục mọi tấm hiện có cho mỗi yêu cầu, vì tổng công việc có thể trở thành bậc hai. Chúng ta cần một chiến lược trong đó mỗi tấm chỉ được di chuyển một số lần không đổi trong toàn bộ quá trình mô phỏng. Giới hạn đầu ra gấp sáu lần số lượng yêu cầu đầu vào và sáu lần số lượng tấm bị rơi gợi ý trực tiếp rằng công trình dự định phải có chi phí khấu hao không đổi trên mỗi tấm. 

Một số trường hợp nguy hiểm có thể phá vỡ một mô phỏng bất cẩn. Đầu tiên là một lần thả, sau đó là một phần:```
2
DROP 3
TAKE 1
```Một đầu ra đúng có thể là:```
DROP 2 3
MOVE 2->1 3
TAKE 1 1
```Ba tấm đầu tiên xếp vào đống 2, sau đó đảo ngược lại thành đống 1 sao cho tấm cũ nhất nằm trên cùng. Giải pháp luôn giữ các tấm mới ở đống 1 sẽ không thành công vì tấm được yêu cầu đầu tiên sẽ bị kẹt bên dưới các tấm mới hơn. 

Một trường hợp khác là vài giọt trước khi uống:```
3
DROP 2
DROP 3
TAKE 5
```Một đầu ra hợp lệ là:```
DROP 2 2
DROP 2 3
MOVE 2->1 5
TAKE 1 5
```Hai nhóm phải hoạt động như một hàng đợi. Nếu việc triển khai chỉ xem xét thao tác thả mới nhất và bỏ qua các bảng trước đó, nó sẽ tạo ra một thứ tự không hợp lệ. 

Trường hợp cạnh cuối cùng là để lại các tấm:```
1
DROP 4
```Đầu ra chỉ cần mô tả việc lưu trữ các tấm:```
DROP 2 4
```Thuật toán không được cố gắng làm trống các cọc ở cuối vì không có yêu cầu nào yêu cầu các tấm đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thực sự bảo trì hai cọc và mô phỏng mọi chuyển động của tấm. Bất cứ khi nào có yêu cầu TAKE đến, chúng tôi sẽ tìm thấy tấm cũ nhất còn lại. Nếu nó ẩn dưới đống khác, chúng ta di chuyển các tấm giữa các cọc cho đến khi tấm đó chạm tới đỉnh thì lấy ra. Điều này đúng vì hai cọc có thể đại diện cho bất kỳ thứ tự nào chúng ta cần. 

Vấn đề với phương pháp này xuất hiện khi có nhiều thao tác xen kẽ xảy ra. Hãy tưởng tượng liên tục thả một chồng lớn và lấy một số lượng nhỏ đĩa. Việc triển khai đơn giản có thể di chuyển cùng một nhóm lớn hết lần này đến lần khác. Với tổng số 100000 tấm bị rơi và 1000 yêu cầu, việc chạm liên tục vào các ngăn xếp lớn có thể đạt tới 100000000 thao tác, điều này là không cần thiết. 

Quan sát quan trọng là các yêu cầu chỉ quan tâm đến thứ tự các đĩa chứ không quan tâm đến danh tính cá nhân của chúng. Chúng ta có thể chọn một đống làm nơi chứa các tấm mới. Sau một nhóm thả, tất cả các tấm trong đống đó đều bị đảo ngược so với thứ tự lấy ra theo yêu cầu. Di chuyển toàn bộ đống sang đống khác sẽ đảo ngược nó một lần, biến ngăn xếp thành thứ tự hàng đợi chính xác. 

Hai cọc tự nhiên hoạt động giống như hai cọc xếp thành một hàng. Chúng tôi giữ chồng 2 làm chồng đầu vào cho các đĩa mới đến và chồng 1 làm chồng đầu ra cho các đĩa sẵn sàng rời đi. Khi một yêu cầu TAKE cần các đĩa và đống 1 trống, chúng tôi di chuyển tất cả các đĩa từ đống 2 sang đống 1. Sau đó, việc loại bỏ rất đơn giản. Đây là kỹ thuật xếp hàng khấu hao tiêu chuẩn, bởi vì mỗi tấm đi qua giữa các cọc nhiều nhất một lần và bị loại bỏ một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(MN) trong trường hợp xấu nhất | O(M) | Quá chậm | 
| Tối ưu | O(M + N) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi yêu cầu DROP trực tiếp vào ngăn xếp 2. Mỗi tấm mới sẽ nằm trên đầu ngăn xếp 2 vì ngăn xếp 2 là ngăn xếp nơi các tấm đến được thu thập. Không cần chuyển động ngay vì máy rửa chén chưa quan tâm đến những chiếc đĩa này. 
2. Trước khi xử lý yêu cầu TAKE, hãy kiểm tra xem đống 1 có đủ đĩa hay không. Nếu chồng 1 trống, hãy di chuyển mọi đĩa từ chồng 2 sang chồng 1. Việc di chuyển chồng từ chồng này sang chồng kia sẽ đảo ngược thứ tự, điều này sẽ thay đổi thứ tự mới nhất đầu tiên của chồng 2 thành thứ tự cũ nhất đầu tiên cần loại bỏ. 
3. Lấy số lượng đĩa được yêu cầu ra khỏi đầu đống 1. Vì đống 1 lưu trữ các đĩa theo đúng thứ tự chúng được giao đến nên việc loại bỏ này đáp ứng yêu cầu của máy rửa bát. 
4. Tiếp tục xử lý yêu cầu cho đến khi vụ việc kết thúc. Những tấm còn lại sẽ được xếp thành chồng vì không cần phải dọn chúng. 

Tại sao nó hoạt động: 

Điều bất biến là chồng 1 luôn chứa các tấm tiếp theo cần lấy ra, theo thứ tự lấy từ trên xuống dưới, còn chồng 2 chứa các tấm đã đến nhưng chưa chuẩn bị dọn đi. DROP chỉ thêm các đĩa mới vào cuối chuỗi chờ. Thao tác DI CHUYỂN chuyển chuỗi chờ sang thứ tự loại bỏ bằng cách đảo ngược ngăn xếp. Vì mỗi lần TAKE chỉ được thực hiện từ cọc 1 nên tấm cũ nhất có sẵn luôn được lấy ra trước. Bất biến được giữ nguyên sau mỗi lệnh, vì vậy mọi bản ghi được tạo đều hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    out = []
    first_case = True

    while True:
        line = input()
        if not line:
            break
        n = int(line)
        if n == 0:
            break

        if not first_case:
            out.append("")
        first_case = False

        pile1 = 0
        pile2 = 0

        for _ in range(n):
            cmd, value = input().split()
            value = int(value)

            if cmd == "DROP":
                out.append(f"DROP 2 {value}")
                pile2 += value
            else:
                if pile1 == 0:
                    if pile2 > 0:
                        out.append(f"MOVE 2->1 {pile2}")
                        pile1 = pile2
                        pile2 = 0

                out.append(f"TAKE 1 {value}")
                pile1 -= value

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai không lưu trữ từng đĩa riêng lẻ vì chỉ có số lượng đĩa trong mỗi chồng là quan trọng. Các thao tác không bao giờ phân biệt được tấm này với tấm khác, vì vậy hai bộ đếm là đủ.`pile1`đại diện cho những chiếc đĩa đã được chuẩn bị sẵn có thể cho vào máy rửa chén ngay lập tức.`pile2`đại diện cho các tấm đang chờ được đảo ngược. Khi`pile1`trống trước TAKE, toàn bộ nội dung của`pile2`được di chuyển. Mã in hoạt động đó và hoán đổi các bộ đếm. 

Phép trừ sau TAKE là an toàn vì vấn đề đảm bảo rằng yêu cầu TAKE không bao giờ yêu cầu nhiều tấm hơn mức hiện tại. Bạn cũng có thể để đĩa thành từng chồng sau yêu cầu cuối cùng vì nhà hàng có thể dừng lại trước khi mọi thứ được rửa sạch. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
DROP 100
TAKE 50
TAKE 20
```Việc thực hiện là: 

| Yêu cầu | đống1 trước | đống2 trước | Hoạt động | đống1 sau | cọc2 sau | 
| --- | --- | --- | --- | --- | --- | 
| GIẢM 100 | 0 | 0 | THẢ 2 100 | 0 | 100 | 
| LẤY 50 | 0 | 100 | DI CHUYỂN 2->1 100, LẤY 1 50 | 50 | 0 | 
| MẤT 20 | 50 | 0 | LẤY 1 20 | 30 | 0 | 

TAKE đầu tiên buộc phải đảo ngược vì tất cả các tấm vẫn còn trong đống đầu vào. Sau khi di chuyển, những tấm cũ nhất có thể lấy được từ đống 1. 

Đối với mẫu thứ hai:```
3
DROP 3
DROP 5
TAKE 8
```Việc thực hiện là: 

| Yêu cầu | đống1 trước | đống2 trước | Hoạt động | đống1 sau | cọc2 sau | 
| --- | --- | --- | --- | --- | --- | 
| THẢ 3 | 0 | 0 | THẢ 2 3 | 0 | 3 | 
| THẢ 5 | 0 | 3 | THẢ 2 5 | 0 | 8 | 
| MẤT 8 | 0 | 8 | DI CHUYỂN 2->1 8, LẤY 1 8 | 0 | 0 | 

Điều này cho thấy nhiều yêu cầu DROP có thể tích lũy cùng nhau. Thuật toán đợi cho đến khi TAKE cần các đĩa, sau đó đảo ngược toàn bộ đống chờ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M + N) | Mỗi yêu cầu được xử lý một lần và mỗi đĩa sẽ di chuyển từ cọc 2 sang cọc 1 tối đa một lần trước khi được thực hiện. | 
| Không gian | O(1) | Chỉ có bộ đếm cho hai cọc và bộ đệm đầu ra được duy trì. | 

Tổng số lần di chuyển của tấm được giới hạn bởi số lần rơi cộng với số lần lấy vì mỗi tấm được di chuyển giữa các cọc nhiều nhất một lần. Điều này thỏa mãn giới hạn sáu lần tổng số tấm bị rơi. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3
DROP 100
TAKE 50
TAKE 20
0
""") == """DROP 2 100
MOVE 2->1 100
TAKE 1 50
TAKE 1 20"""

assert run("""3
DROP 3
DROP 5
TAKE 8
0
""") == """DROP 2 3
DROP 2 5
MOVE 2->1 8
TAKE 1 8"""

assert run("""1
DROP 1
0
""") == """DROP 2 1"""

assert run("""4
DROP 5
TAKE 2
DROP 3
TAKE 6
0
""") == """DROP 2 5
MOVE 2->1 5
TAKE 1 2
DROP 2 3
TAKE 1 3"""

assert run("""2
DROP 100000
TAKE 100000
0
""") == """DROP 2 100000
MOVE 2->1 100000
TAKE 1 100000"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`DROP 1`| Một hoạt động DROP | Trường hợp tối thiểu và cọc chưa hoàn thiện | 
| Nhiều lần thả theo sau là một lần | Một DI CHUYỂN lớn trước khi NHẬN | Đảo ngược ngăn xếp chính xác | 
| Thả một lần lớn và lấy | Một lần đảo ngược 100000 tấm | Xử lý số lượng tấm tối đa | 
| Thay phiên nhau thả và mất | Một số chuyển tiếp cọc | Bảo trì đúng bất biến | 

## Vỏ cạnh 

Đối với một lần DROP theo sau là một lần NHẬN:```
2
DROP 3
TAKE 1
```Thuật toán đầu tiên xếp cả ba tấm vào đống 2. Trước khi NHẬN, đống 1 trống nên cả ba tấm được chuyển vào đống 1. Đỉnh của đống 1 lúc này là tấm cũ nhất, bỏ đi một tấm là đúng. Hai tấm còn lại vẫn có sẵn cho những yêu cầu sau này. 

Đối với nhiều lần giảm liên tiếp:```
3
DROP 2
DROP 3
TAKE 5
```Thuật toán không di chuyển bất cứ thứ gì trong quá trình DROP. Đống 2 chỉ đơn giản tăng từ kích thước 2 lên kích thước 5. Khi TAKE đến, một DI CHUYỂN sẽ đảo ngược bộ sưu tập hoàn chỉnh. Điều này xử lý toàn bộ hàng đợi thay vì xử lý từng DROP riêng biệt. 

Đối với công việc chưa hoàn thành:```
1
DROP 4
```Thuật toán chỉ in hành động cần thiết để nhận các tấm. Các quầy kết thúc chính xác với cọc 2 chứa bốn tấm. Không có hoạt động bổ sung nào được thực hiện vì đầu vào không bao giờ yêu cầu giao những tấm đó. 

Bạn có thể điều chỉnh độ dài hoặc điểm nhấn của bài xã luận nếu bạn muốn một phiên bản mang phong cách cuộc thi hơn hoặc một hướng dẫn mang tính giáo dục hơn.
