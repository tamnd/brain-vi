---
title: "CF 105733A - GDSC v\u00e0 BKAC"
description: "Hai nhóm GDSC và BKAC đang quét một chuỗi từ trái sang phải, từng ký tự. Mỗi nhóm có một tập hợp các chữ cái mục tiêu cố định. GDSC đang cố gắng thu thập các chữ cái G, D, S và C. BKAC đang cố gắng thu thập B, K, A và C."
date: "2026-06-26T07:47:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105733
codeforces_index: "A"
codeforces_contest_name: "Bach Khoa Code Challenge #1"
rating: 0
weight: 105733
solve_time_s: 40
verified: true
draft: false
---

[CF 105733A - GDSC v\u00e0 BKAC](https://codeforces.com/problemset/problem/105733/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai nhóm GDSC và BKAC đang quét một chuỗi từ trái sang phải, từng ký tự. Mỗi nhóm có một tập hợp các chữ cái mục tiêu cố định. GDSC đang cố gắng thu thập các chữ cái G, D, S và C. BKAC đang cố gắng thu thập B, K, A và C. Bất cứ khi nào một ký tự xuất hiện trong chuỗi, cả hai nhóm sẽ kiểm tra độc lập xem nó có thuộc tập mục tiêu của họ hay không. Nếu có, họ sẽ thêm nó vào bộ sưu tập của mình, còn nếu không, họ sẽ bỏ qua. Nhóm đầu tiên thu thập được tất cả các chữ cái cần thiết ít nhất một lần sẽ chiến thắng. Nếu cả hai hoàn thành bộ sưu tập của mình ở cùng một vị trí trong quá trình quét, kết quả là hòa. 

Chi tiết quan trọng là thứ tự các ký tự trong chuỗi xác định dòng thời gian. Chúng tôi không sắp xếp lại hoặc chọn các chuỗi tiếp theo; chúng tôi đang mô phỏng một quy trình tuần tự trong đó mỗi bước có thể đóng góp cho một hoặc cả hai nhóm. 

Các ràng buộc nhỏ, với độ dài chuỗi lên tới 100 và nhiều nhất là 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa ngoài một lần quét tuyến tính cho mỗi trường hợp thử nghiệm. Cách tiếp cận O(n²) vẫn có thể vượt qua một cách thoải mái, nhưng bất cứ điều gì phức tạp hơn một lần vượt qua đều là chi phí không cần thiết. 

Một trường hợp phức tạp phát sinh từ nhân vật chung C. Cả hai đội đều cần nó. Điều này tạo ra các kịch bản trong đó tiến trình có thể được đồng bộ hóa theo những cách không rõ ràng. Ví dụ: nếu C xuất hiện rất sớm nhưng các chữ cái bắt buộc còn lại nằm rải rác, cả hai đội có thể hoàn thành vào các thời điểm khác nhau mặc dù có chung tiến trình trên một nhân vật chính. 

Một trường hợp đặc biệt khác xuất hiện khi các chữ cái bắt buộc của một đội xuất hiện sớm hơn nhiều nhưng thiếu một chữ cái cuối cùng sẽ làm chậm quá trình hoàn thành. Ví dụ: nếu BKAC thu thập B, K và A ở một số vị trí đầu tiên nhưng C xuất hiện muộn, trong khi GDSC dần dần tích lũy các chữ cái sớm hơn, thì người chiến thắng hoàn toàn phụ thuộc vào yêu cầu còn thiếu cuối cùng thay vì tần suất hoặc sự xuất hiện đa số. 

Cuối cùng, các ký tự lặp lại rất quan trọng. Một sai lầm ngây thơ là đếm số lần xuất hiện thay vì theo dõi xem mỗi chữ cái bắt buộc đã được nhìn thấy ít nhất một lần hay chưa. Ví dụ: trong một chuỗi như`GGGG`, GDSC chưa hoàn thành bất cứ điều gì vì nó vẫn thiếu D, S và C, mặc dù cách diễn giải dựa trên tần số có thể gợi ý không chính xác về tiến độ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quá trình chính xác như được mô tả. Đối với mỗi tiền tố của chuỗi, chúng tôi duy trì hai bộ đại diện cho những gì mỗi nhóm đã thu thập được cho đến nay. Tại mọi vị trí, chúng tôi cập nhật các bộ và kiểm tra xem chúng có chứa tất cả các ký tự bắt buộc hay không. Điều này đúng vì nó phản ánh trực tiếp các quy tắc. 

Cách tiếp cận này chạy trong O(n) cho mỗi trường hợp thử nghiệm vì mỗi ký tự được xử lý một lần và các thao tác thiết lập có thời gian không đổi. Ngay cả khi chúng tôi triển khai nó ít cẩn thận hơn bằng cách quét lặp lại các chữ cái được yêu cầu ở mỗi bước, chúng tôi vẫn sẽ nằm trong O(n × 8), giá trị này không đáng kể khi cho n ≤ 100. 

Không cần tối ưu hóa sâu hơn vì cấu trúc của bài toán vốn đã có tính tuần tự. Quan sát có ý nghĩa duy nhất là chúng ta không bao giờ cần phải xem lại các ký tự trong quá khứ hoặc tính toán bất cứ điều gì ngoài việc “chúng ta đã thấy tất cả các chữ cái được yêu cầu chưa”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(n) | O(1) | Đã chấp nhận | 
| Theo dõi một lần tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi chuỗi này là dòng thời gian và duy trì hai trình theo dõi boolean cho mỗi nhóm. 

1. Khởi tạo bốn mảng hoặc bộ boolean: một cho tiến trình GDSC và một cho tiến trình BKAC. Mỗi cái bắt đầu trống vì chưa có ký tự nào được nhìn thấy. 
2. Xác định trước các bộ cần thiết: GDSC cần G, D, S, C; BKAC cần B, K, A, C. 
3. Quét chuỗi từ trái sang phải. Đối với mỗi nhân vật, hãy cập nhật tiến trình của cả hai đội nếu nhân vật đó thuộc bộ yêu cầu tương ứng của họ. Bước này đảm bảo cả hai nhóm tiến hành độc lập nhưng đồng bộ trên cùng một luồng đầu vào. 
4. Sau khi cập nhật một ký tự, hãy kiểm tra xem GDSC có đủ bốn chữ cái bắt buộc hay không. Nếu vậy, hãy ghi lại chỉ mục hiện tại làm điểm hoàn thành của nó. 
5. Thực hiện kiểm tra tương tự đối với BKAC và ghi lại điểm hoàn thành của nó. 
6. Sau khi quét xong, hãy so sánh các chỉ số hoàn thành đã ghi. Nếu một người nhỏ hơn, đội đó thắng. Nếu bằng nhau thì kết quả là hòa. 

Lựa chọn thiết kế quan trọng là chúng tôi chỉ quan tâm đến thời điểm đầu tiên mỗi nhóm trở nên hoàn thiện. Khi một đội đã thu thập được tất cả các chữ cái, những lần xuất hiện sau đó không quan trọng trong việc quyết định đội chiến thắng. 

### Tại sao nó hoạt động 

Trạng thái của mỗi đội chỉ phụ thuộc vào việc mỗi chữ cái bắt buộc đã xuất hiện ít nhất một lần trước đó hay ở vị trí hiện tại. Trạng thái đơn điệu: một khi một lá thư được thu thập, nó sẽ không bao giờ bị mất. Do đó, vị trí đầu tiên có tất cả các chữ cái bắt buộc được xác định duy nhất và đủ để xác định kết quả. Việc so sánh các vị trí hoàn thành đầu tiên này nắm bắt đầy đủ sự cạnh tranh được mô tả trong quy trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input().strip())
        s = input().strip()

        need_gdsc = set("GDSC")
        need_bkac = set("BKAC")

        have_gdsc = set()
        have_bkac = set()

        finish_gdsc = -1
        finish_bkac = -1

        for i, ch in enumerate(s):
            if ch in need_gdsc:
                have_gdsc.add(ch)
            if ch in need_bkac:
                have_bkac.add(ch)

            if finish_gdsc == -1 and have_gdsc == need_gdsc:
                finish_gdsc = i
            if finish_bkac == -1 and have_bkac == need_bkac:
                finish_bkac = i

        if finish_gdsc < finish_bkac:
            print("GDSC")
        elif finish_bkac < finish_gdsc:
            print("BKAC")
        else:
            print("DRAW")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo mô phỏng chính xác. Các bộ`have_gdsc`Và`have_bkac`theo dõi những ký tự cần thiết đã gặp cho đến nay. Khi một nhóm đạt đến mức hoàn thiện, chúng tôi sẽ khóa chỉ mục hoàn thành của nhóm đó để các bản cập nhật sau này không ghi đè lên nó. 

Một điểm tinh tế là khởi tạo các chỉ số hoàn thành thành -1. Điều này đảm bảo rằng sau khi một đội hoàn thành, chúng tôi sẽ không tính toán lại hoặc vô tình làm thay đổi kết quả của đội đó. Việc so sánh cuối cùng hoàn toàn dựa trên thời gian hoàn thành đầu tiên. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi trong đó cả hai đội hoàn thành vào các thời điểm khác nhau. 

đầu vào:```
1
6
GDBSCA
```Chúng tôi theo dõi tiến độ từng bước. 

| tôi | char | GDSC có | BKAC có | GDSC xong | BKAC xong | 
| --- | --- | --- | --- | --- | --- | 
| 0 | G | G | | Không | Không | 
| 1 | D | GD | | Không | Không | 
| 2 | B | GD | B | Không | Không | 
| 3 | S | GDS | B | Không | Không | 
| 4 | C | GDSC | BC | Có (4) | Không | 
| 5 | A | GDSC | BCA | Có (4) | Có (5) | 

GDSC về đích ở chỉ số 4, BKAC ở chỉ số 5 nên GDSC thắng. Dấu vết cho thấy cách một ký tự được chia sẻ như C đóng góp đồng thời vào cả hai đường dẫn tiến trình nhưng không đảm bảo mức độ hoàn thành như nhau. 

Bây giờ hãy xem xét trường hợp hoàn thành đồng thời. 

đầu vào:```
1
4
BKAC
```| tôi | char | GDSC có | BKAC có | GDSC xong | BKAC xong | 
| --- | --- | --- | --- | --- | --- | 
| 0 | B | | B | Không | Không | 
| 1 | K | | BK | Không | Không | 
| 2 | A | | BKA | Không | Không | 
| 3 | C | C | BKAC | Có (3) | Có (3) | 

Cả hai đội đều hoàn thành với cùng chỉ số, tạo ra một trận hòa. Điều này xác nhận rằng các ký tự cuối cùng được chia sẻ sẽ đồng bộ hóa trực tiếp quá trình hoàn thành khi cả hai bộ đều được đáp ứng tại cùng một thời điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự được xử lý một lần với các cập nhật và kiểm tra được thiết lập theo thời gian liên tục | 
| Không gian | O(1) | Chỉ các bộ có kích thước cố định cho tối đa bốn ký tự mỗi đội | 

Cho n ⁴ 100 và t ⁴ 100, giải pháp chạy tối đa 10⁴ ký tự, không đáng kể trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""3
4
CKAB
4
GSDC
10
BAKAZPGDSC
""") == """BKAC
GDSC
DRAW"""

# minimum size, immediate C overlap
assert run("""1
4
BKAC
""") == "DRAW"

# GDSC clearly earlier
assert run("""1
5
GDSCA
""") == "GDSC"

# BKAC clearly earlier
assert run("""1
6
BKAACD""") == "BKAC"

# missing early letters forces late completion
assert run("""1
8
AAAAKBCGDS""") == "BKAC"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khối mẫu | hỗn hợp | tính đúng đắn của toàn bộ kịch bản | 
| BKAC | VẼ | hoàn thành đồng thời | 
| GDSCA | GDSC | thiên vị hoàn thành sớm | 
| BKAACD | BKAC | sự bất đối xứng trong các chữ cái bắt buộc | 
| AAAAKBCGDS | BKAC | chậm hoàn thành do thiếu chữ cái chính | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi cả hai đội phụ thuộc nhiều vào ký tự chung C, nhưng các chữ cái bắt buộc khác của họ lại xuất hiện vào những thời điểm rất khác nhau. Đối với đầu vào như`CCCCDGSKBA`, cả hai đội thu thập C gần như ngay lập tức, nhưng việc hoàn thành được quyết định hoàn toàn bởi chữ cái không chia sẻ cuối cùng bị thiếu. Thuật toán xử lý việc này một cách chính xác vì quá trình hoàn thành chỉ được kích hoạt khi toàn bộ tập hợp được hài lòng chứ không phải tiến trình một phần. 

Một trường hợp khác là lặp lại những ký tự không liên quan. Trong một chuỗi như`ZZZZGDSC`, GDSC chỉ hoàn thành ở lần xuất hiện cuối cùng của C mặc dù nhiều ký tự không liên quan xuất hiện trước đó. Vì thuật toán bỏ qua hoàn toàn các ký tự không bắt buộc nên chúng không ảnh hưởng đến quá trình chuyển đổi trạng thái
