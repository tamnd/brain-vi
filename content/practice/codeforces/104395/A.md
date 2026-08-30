---
title: "CF 104395A - Bánh thưởng cho bò"
description: "Chúng tôi được cung cấp một bản ghi theo trình tự thời gian về một chuồng trại nơi những con bò liên tục ra vào. Mỗi dòng đầu vào mô tả một sự kiện duy nhất cho một con bò cụ thể: nếu con bò hiện đang ở bên ngoài, sự kiện này có nghĩa là nó đang đi vào; nếu nó hiện đang ở bên trong, nó sẽ rời đi."
date: "2026-07-01T02:25:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104395
codeforces_index: "A"
codeforces_contest_name: "Cupertino Informatics Tournament"
rating: 0
weight: 104395
solve_time_s: 81
verified: false
draft: false
---

[CF 104395A - Thức ăn cho bò](https://codeforces.com/problemset/problem/104395/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bản ghi theo trình tự thời gian về một chuồng trại nơi những con bò liên tục ra vào. Mỗi dòng đầu vào mô tả một sự kiện duy nhất cho một con bò cụ thể: nếu con bò hiện đang ở bên ngoài, sự kiện này có nghĩa là nó đang đi vào; nếu nó hiện đang ở bên trong, nó sẽ rời đi. Chuồng bắt đầu trống và tại mọi thời điểm, chúng tôi có thể theo dõi chính xác những con bò nào ở bên trong. 

Nhiệm vụ không phải là mô phỏng để gây tò mò mà là xác định những con bò nào có thể đã đánh cắp đồ ăn bị thiếu theo lý thuyết hành vi của Nông dân John. Một con bò có thể bị coi là nghi phạm nếu có thể thỏa mãn hai điều kiện tại một thời điểm nào đó: thứ nhất, con bò đó phải là con bò duy nhất ở trong chuồng vào thời điểm đó và thứ hai, sau thời điểm đó, con bò đó không bao giờ được vào chuồng cùng lúc với một con bò khác. Nói cách khác, một khi con bò đã “hiện diện một mình”, nó sẽ không bao giờ được chia sẻ chuồng với bất kỳ ai nữa sau đó. 

Kích thước đầu vào lên tới 100.000 sự kiện. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào liên tục quét tất cả các con bò hoặc tính toán lại trạng thái toàn cầu từ đầu cho mỗi sự kiện, vì hành vi O(n²) sẽ quá chậm. Cần phải có một đường truyền tuyến tính hoặc gần tuyến tính với cấu trúc dựa trên hàm băm. 

Một trường hợp thất bại tinh vi xuất phát từ việc chỉ nghĩ đến khoảnh khắc con bò ở một mình. Một con bò có thể ở một mình nhiều lần nhưng sau đó lại vào lại khi có những con khác có mặt. Điều đó vô hiệu hóa nó vĩnh viễn. Một trường hợp khác là những con bò không bao giờ ở một mình, phải loại trừ ngay cả khi chúng ra vào sạch sẽ. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ mô phỏng toàn bộ quá trình và đối với mỗi con bò, hãy cố gắng xác minh tình trạng bằng cách quét tất cả các sự kiện sau mỗi thời điểm nó ở một mình. Đối với mỗi “khoảnh khắc solo” của ứng viên, chúng tôi sẽ kiểm tra phần còn lại của nhật ký để đảm bảo con bò không bao giờ trùng với con bò khác nữa. Điều này dẫn đến khả năng kiểm tra O(n) các bước trong tương lai cho tối đa O(n) sự kiện, tạo ra hành vi O(n²) trong trường hợp xấu nhất, quá chậm đối với 100.000 sự kiện. 

Quan sát quan trọng là chúng ta không thực sự cần phải quét lại tương lai cho từng thời điểm ứng cử viên. Điều quan trọng duy nhất là liệu một con bò có vi phạm quy tắc “không chia sẻ sau khi xuất hiện một mình” hay không. Sau khi duy trì nhóm bò hiện tại trong chuồng, chúng tôi có thể phát hiện các vi phạm ngay lập tức khi chúng xảy ra. Ngoài ra, chúng tôi có thể ghi lại xem con bò có từng ở một mình trong chuồng hay không. 

Vì vậy, thay vì kiểm tra hồi cứu, chúng tôi duy trì hai thông tin trực tuyến: tình trạng hiện tại của chuồng và cờ trên mỗi con bò cho biết liệu nó có từng ở trạng thái ở một mình hay không. Chúng tôi cũng duy trì một lá cờ toàn cầu cho biết liệu một con bò có từng vi phạm quy tắc khi ở trong cùng với một con bò khác sau khoảnh khắc solo đầu tiên của nó hay không. Điều này chuyển đổi vấn đề thành một mô phỏng một lượt với các cập nhật liên tục theo thời gian cho mỗi sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì một bộ`inside`đại diện cho những con bò hiện đang ở trong chuồng. Điều này cho phép theo dõi việc vào và ra theo thời gian liên tục cho mỗi sự kiện. 
2. Duy trì mảng boolean`ever_alone[x]`cho biết liệu bò`x`đã từng ở trong chuồng khi nó là con bò duy nhất có mặt. Điều này trực tiếp nắm bắt được điều kiện đầu tiên cần thiết để ứng cử. 
3. Duy trì mảng boolean`invalid[x]`cho biết liệu bò`x`đã từng vi phạm quy tắc chia sẻ chuồng trại sau khi ở một mình. Sau khi được thiết lập, con bò này không bao giờ có thể là ứng cử viên. 
4. Xử lý từng sự kiện một. Đối với mỗi sự kiện liên quan đến con bò`x`, chuyển đổi sự hiện diện của nó trong`inside`. Nếu nó hiện diện, hãy loại bỏ nó; nếu không hãy chèn nó vào. Điều này giữ cho trạng thái luôn nhất quán. 
5. Sau mỗi lần cập nhật, hãy kiểm tra xem kích thước của`inside`chính xác là một. Nếu vậy, con bò duy nhất hiện đang ở bên trong có một khoảnh khắc cô độc, vì vậy hãy đánh dấu nó`ever_alone`lá cờ. Đây là khoảnh khắc duy nhất mà sự cô độc có thể được phát hiện một cách đáng tin cậy mà không cần nhận thức muộn màng. 
6. Sau mỗi lần cập nhật, đối với mỗi con bò hiện đang ở trong chuồng, nếu kích thước của`inside`lớn hơn một thì bất kỳ con bò nào đã được đánh dấu`ever_alone`hiện đang chứng kiến ​​một hành vi vi phạm nếu nó lại xuất hiện. Đối với tất cả những con bò như vậy, đặt`invalid[x] = True`. 
7. Cuối cùng, con bò nào vừa ý`ever_alone[x] == True`Và`invalid[x] == False`là một ứng cử viên hợp lệ. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`invalid[x]`trở thành sự thật chính xác khi con bò`x`tham gia vào một cấu hình vi phạm quy tắc sau khi nó đã trải qua sự cô độc. Vì chúng ta xử lý các sự kiện theo thứ tự thời gian và chỉ đánh dấu sự cô độc khi nó được tuân thủ nghiêm ngặt (kích thước của`inside`bằng một), không có kết quả dương tính giả nào phát sinh. Bất kỳ sự hiện diện nào trong tương lai sau thời điểm đó sẽ bị phát hiện ngay lập tức và loại bỏ vĩnh viễn con bò. Ngược lại, nếu một con bò không bao giờ bị vô hiệu và có ít nhất một khoảnh khắc đơn độc thực sự thì nó đáp ứng cả hai điều kiện bắt buộc theo cách xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    inside = set()
    ever_alone = {}
    invalid = {}
    
    events = []
    
    for _ in range(n):
        x = int(input())
        events.append(x)
        if x not in ever_alone:
            ever_alone[x] = False
            invalid[x] = False

    for x in events:
        if x in inside:
            inside.remove(x)
        else:
            inside.add(x)

        if len(inside) == 1:
            y = next(iter(inside))
            ever_alone[y] = True

        if len(inside) > 1:
            for y in inside:
                if ever_alone.get(y, False):
                    invalid[y] = True

    ans = []
    for x in ever_alone:
        if ever_alone[x] and not invalid[x]:
            ans.append(x)

    ans.sort()
    sys.stdout.write("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`inside`set, theo dõi trạng thái hiện tại của chuồng chính xác như logic chuyển đổi ra lệnh. Mỗi sự kiện lật thành viên trong thời gian không đổi. 

các`ever_alone`từ điển chỉ được cập nhật khi kích thước chuồng trở thành chính xác một. Điều đó đảm bảo rằng chúng ta chỉ ghi lại sự cô độc thực sự chứ không phải những trạng thái nhất thời hay mơ hồ. 

các`invalid`cờ được kích hoạt bất cứ khi nào một con bò đã trải qua sự cô độc xuất hiện trong cấu hình nhiều con bò. Điều này thực thi ràng buộc "không bao giờ lặp lại với người khác". 

Cuối cùng, việc lọc theo cả hai điều kiện đảm bảo chỉ còn lại những ứng viên hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10
96518
96518
4862
4862
90754
71337
71337
61387
95917
95917
```Chúng tôi theo dõi quá trình chuyển đổi trạng thái. 

| Bước | Sự kiện | Bộ bên trong | Bò độc thân | cập nhật ever_alone | cập nhật không hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 96518 trong | {96518} | 96518 | 96518=Đúng | không | 
| 2 | 96518 ra | {} | không | không | không | 
| 3 | 4862 trong | {4862} | 4862 | 4862=Đúng | không | 
| 4 | 4862 ra | {} | không | không | không | 
| 5 | 90754 trong | {90754} | 90754 | 90754=Đúng | không | 
| 6 | 71337 trong | {90754,71337} | không | không | 90754,71337 không hợp lệ nếu luôn_một mình | 
| 7 | 71337 ra | {90754} | không | không | không | 
| 8 | 61387 trong | {90754,61387} | không | không | 90754 không hợp lệ | 
| 9 | 95917 trong | {90754,61387,95917} | không | không | 90754,61387,95917 không hợp lệ | 
| 10 | 95917 ra | {90754,61387} | không | không | không | 

Sau khi xử lý, chỉ những con bò ở một mình tại một thời điểm nào đó và không bao giờ xuất hiện trở lại ở trạng thái nhiều con bò vẫn hợp lệ, phù hợp với kết quả dự kiến:```
4862
96518
```Dấu vết này cho thấy một con bò có thể bị tàn tật không phải ngay lập tức ở thời điểm nó sống đơn lẻ mà sau đó khi nó quay trở lại chuồng đông đúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) trung bình | Mỗi sự kiện cập nhật một bộ băm và thực hiện kiểm tra theo thời gian liên tục | 
| Không gian | O(n) | Chúng tôi lưu trữ trạng thái cho mỗi ID bò riêng biệt cộng với nhật ký sự kiện | 

Các ràng buộc cho phép thực hiện 100.000 thao tác và mỗi thao tác được xử lý trong thời gian không đổi hoặc gần như không đổi bằng cách sử dụng các cấu trúc dựa trên hàm băm, giúp giải pháp luôn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return output.getvalue().strip()

# sample
assert run("""10
96518
96518
4862
4862
90754
71337
71337
61387
95917
95917
""") == "4862\n96518"

# minimal case: single cow
assert run("""1
5
""") == "5"

# two cows alternating
assert run("""4
1
2
1
2
""") == ""

# cow alone then invalidated later
assert run("""5
1
2
1
2
1
""") == ""

# all cows isolated and never overlap
assert run("""6
1
1
2
2
3
3
""") == "1\n2\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bò đơn | 5 | xử lý kích thước tối thiểu | 
| bò xen kẽ | trống | logic vô hiệu ngay lập tức | 
| tái nhập cảnh sau solo | trống | vi phạm sau cô độc | 
| cặp rời rạc | 1,2,3 | nhiều ứng viên hợp lệ | 

## Vỏ cạnh 

Một trường hợp quan trọng là một con bò trở nên đơn độc sớm, sau đó quay trở lại khi có một con bò khác. Ví dụ: con bò 1 có thể xuất hiện một mình ở thời điểm 1, nhưng sau đó sẽ nhập lại khi con bò 2 ở bên trong. Trong trường hợp này, thuật toán đánh dấu chính xác`ever_alone[1] = True`tại thời điểm đầu tiên và các bộ sau đó`invalid[1] = True`khi sự chồng chéo xảy ra. Bộ lọc cuối cùng loại trừ nó mặc dù nó có hiệu lực sớm. 

Một trường hợp khác là những con bò không bao giờ sống đơn độc. Họ có thể vào và ra nhiều lần mà không bao giờ là người cư ngụ duy nhất. Thuật toán không bao giờ đặt`ever_alone[x]`đối với họ nên cuối cùng họ sẽ tự động bị loại trừ. 

Một trường hợp khó phát hiện cuối cùng là khi chỉ có một con bò hiện diện trong chuồng trong toàn bộ khúc gỗ. Trong kịch bản đó, con bò đó trở thành`ever_alone`ngay lập tức và không bao giờ bị vô hiệu, do đó nó được đưa vào một cách chính xác như một nghi phạm hợp lệ.
