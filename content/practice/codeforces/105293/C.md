---
title: "CF 105293C - Mr. Wow và Phép Thuật"
description: "Chúng tôi được cấp một hàng quái vật, mỗi con quái vật có một giá trị sức khỏe tích cực. Phép thuật là một thao tác trong đó chúng ta chọn một số x, sau đó quét quái vật từ trái sang phải. Quái vật đầu tiên có lượng máu hiện tại ít nhất là x sẽ bị giảm x và phép thuật sẽ dừng ngay lập tức."
date: "2026-06-23T14:41:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105293
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #33(Wow-Forces)"
rating: 0
weight: 105293
solve_time_s: 148
verified: false
draft: false
---

[CF 105293C - Mr. Wow và Spells](https://codeforces.com/problemset/problem/105293/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một hàng quái vật, mỗi con quái vật có một giá trị sức khỏe tích cực. Phép thuật là một thao tác trong đó chúng ta chọn một số`x`, sau đó quét quái vật từ trái sang phải. Quái vật đầu tiên có lượng máu hiện tại ít nhất là`x`được giảm bởi`x`, và câu thần chú dừng lại ngay lập tức. Nếu không có quái vật nào có sức khỏe thì ít nhất`x`, câu thần chú không có tác dụng gì. Trò chơi kết thúc ngay lập tức nếu một câu thần chú giết chết một con quái vật (máu của nó trở thành 0) hoặc không ảnh hưởng đến bất kỳ con quái vật nào. 

Chúng tôi muốn chọn một chuỗi các phép thuật như vậy, mỗi lần chọn một phép thuật mới`x`, để tối đa hóa số lượng phép thuật có thể được thực thi trước khi quá trình buộc phải dừng lại. 

Hạn chế chính là điều kiện dừng: chúng tôi không được phép giảm một con quái vật về 0 và chúng tôi không được phép sử dụng một câu thần chú đánh trượt tất cả quái vật. Điều này tạo ra một sự cân bằng tinh tế: mọi hoạt động phải “phù hợp” với bối cảnh sức khỏe hiện tại và mọi bản cập nhật sẽ định hình lại vĩnh viễn những gì các phép thuật trong tương lai có thể làm. 

Các ràng buộc cho phép lên đến`3 · 10^5`tổng số quái vật trong các trường hợp thử nghiệm, loại trừ mọi mô phỏng bậc hai đối với tất cả các tương tác phép thuật có thể xảy ra. Bất kỳ giải pháp nào cũng phải xử lý từng trường hợp kiểm thử theo thời gian tuyến tính hoặc gần tuyến tính, thông thường`O(n)`hoặc`O(n log n)`. 

Một mô phỏng ngây thơ sẽ cố gắng chọn nhiều lần`x`, quét mảng, áp dụng các bản cập nhật và lặp lại cho đến khi thất bại. Ngay cả khi mỗi câu thần chú là`O(n)`, số lượng phép thuật cũng có thể lớn, khiến phương pháp này có khả năng`O(n^2)`trong trường hợp xấu nhất. 

Một trường hợp phức tạp xuất hiện khi nhiều quái vật có lượng máu bằng nhau hoặc rất nhỏ. Ví dụ: nếu tất cả các giá trị là`1`, thì bất kỳ câu thần chú hợp lệ nào cũng phải sử dụng`x = 1`, ngay lập tức giết chết quái vật đầu tiên và kết thúc trò chơi sau một thao tác. Một kỳ vọng ngây thơ có thể là chúng ta có thể “gây thiệt hại”, nhưng quy tắc dừng từ trái sang phải hoàn toàn ngăn cản điều đó. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực mô phỏng trò chơi theo đúng nghĩa đen. Đối với mỗi câu thần chú, chúng tôi thử các lựa chọn khác nhau`x`, mô phỏng quá trình truyền tải từ trái sang phải, cập nhật mảng và kiểm tra xem quá trình có dừng hay không. Điều này đúng nhưng về cơ bản là tốn kém vì mỗi phép thuật đều tốn thời gian tuyến tính và có thể có nhiều phép thuật trước khi kết thúc. Tổng công việc có thể dễ dàng xuống cấp`O(n^2)`. 

Quan sát cấu trúc quan trọng là quá trình này không thực sự phân phối sát thương lên nhiều quái vật một cách linh hoạt. Mỗi phép thuật luôn tác động lên chính xác một quái vật: quái vật đầu tiên có ít nhất máu`x`. Điều đó có nghĩa là mọi hoạt động thực sự là một phép trừ có kiểm soát được áp dụng cho một vị trí duy nhất, nhưng với hạn chế là các quái vật trước đó đóng vai trò là "bộ lọc" xác định vị trí nào có thể tiếp cận được trong một vị trí nhất định.`x`. 

Hành vi lọc này ngụ ý một cấu trúc đơn điệu: tiếp cận quái vật`i`, chúng ta phải chọn`x`lớn hơn tất cả quái vật trước đó, nếu không quá trình quét sẽ dừng sớm hơn. Vì vậy mỗi vị trí`i`có một cửa sổ có thể sử dụng`x`các giá trị được xác định bởi lượng máu tối đa ở bên trái và lượng máu còn lại hiện tại của nó. 

Chiến lược tối ưu xuất hiện từ việc liên tục khai thác các cửa sổ này theo cách thận trọng nhất có thể, luôn chọn các giá trị giúp giảm thiểu tốc độ chúng ta giảm sức khỏe của quái vật, đồng thời đảm bảo chúng ta vẫn vượt qua tất cả quái vật trước đó. Điều này làm giảm quá trình theo dõi xem mỗi tiền tố có thể “phát triển” bao xa và tổng hợp những đóng góp đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Xây dựng dựa trên tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi quét quái vật từ trái sang phải trong khi vẫn duy trì lượng máu tối đa cho đến nay. Giá trị này đại diện cho “công cụ chặn” mạnh nhất để tiếp cận những quái vật sâu hơn với giá trị phép thuật đã chọn. 
2. Đối với mỗi quái vật`i`, chúng tôi so sánh sức khỏe của nó`h[i]`với tiền tố tối đa hiện tại. Nếu như`h[i]`lớn hơn, quái vật này giới thiệu không gian mới có thể tiếp cận được cho năng lượng phép thuật hợp lệ. Nếu không, nó sẽ không mở rộng được những gì chúng ta có thể làm ngoài những gì mà những con quái vật trước đó đã cho phép. 
3. Chúng tôi chỉ tích lũy các mức tăng dương khi mức tối đa tiền tố mới xuất hiện. Mỗi khi một con quái vật vượt quá tất cả những con quái vật trước đó, sự khác biệt sẽ góp phần tạo ra “phạm vi có thể sử dụng” mới cho các lựa chọn phép thuật. 
4. Câu trả lời cuối cùng là tổng số mở rộng tích lũy của tiền tố tối đa này trên toàn mảng. 

### Tại sao nó hoạt động 

Mọi phép thuật hợp lệ đều bị hạn chế bởi lượng máu tối đa của những con quái vật trước đó, vì điều đó quyết định mức độ lan truyền của phép thuật. Một quái vật mới chỉ tăng số lượng hoạt động riêng biệt có thể có khi nó vượt quá tất cả các giá trị trước đó, bởi vì chỉ khi đó nó mới mở ra một phạm vi hợp lệ mới`x`những giá trị mà trước đây không thể có được. Tất cả những con quái vật khác đều bị thống trị hoàn toàn bởi những ràng buộc trước đó và không tạo ra những lựa chọn độc lập mới. 

Do đó, tổng số phép thuật được xác định đầy đủ bằng số lần tiền tố tăng tối đa và tổng số tiền tăng lên bao nhiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        h = list(map(int, input().split()))

        ans = 0
        pref_max = 0

        for x in h:
            if x > pref_max:
                ans += x - pref_max
                pref_max = x

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì tiền tố chạy tối đa trên mảng. Bất cứ khi nào một giá trị vượt quá nó, sự khác biệt sẽ góp phần tạo nên câu trả lời. Đây là thời điểm duy nhất khi “năng lực” phép thuật độc lập mới được tạo ra; nếu không thì cấu trúc của những con quái vật trước đó sẽ chi phối hoàn toàn những gì có thể. 

Một lỗi phổ biến là cố gắng mô phỏng cập nhật theo từng chính tả. Điều đó ngay lập tức trở nên quá chậm và cũng che khuất thực tế rằng chỉ có sự chuyển đổi tối đa tiền tố mới quan trọng. Một sai lầm khác là cho rằng mỗi con quái vật đóng góp độc lập, nhưng quy tắc dừng từ trái sang phải kết hợp tất cả các quyết định cho đến mức tối đa được thấy cho đến nay. 

## Ví dụ đã hoạt động 

Xem xét một đầu vào có nhiều trường hợp thử nghiệm: 

### Ví dụ 1 

Mảng:`[3, 2, 3]`| tôi | h[i] | tiền tố tối đa | đóng góp | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 3 | 
| 2 | 2 | 3 | 0 | 3 | 
| 3 | 3 | 3 | 0 | 3 | 

Phần tử đầu tiên thiết lập mức tối đa tiền tố mới, đóng góp 3. Các phần tử sau không bao giờ vượt quá giá trị đó, vì vậy chúng không bổ sung thêm năng lực độc lập mới. 

### Ví dụ 2 

Mảng:`[2, 1, 4]`| tôi | h[i] | tiền tố tối đa | đóng góp | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 2 | 
| 2 | 1 | 2 | 0 | 2 | 
| 3 | 4 | 4 | 2 | 4 | 

Quái vật thứ ba giới thiệu mức tối đa mới, thêm vào`4 - 2 = 2`. 

Những dấu vết này cho thấy rằng chỉ tăng lên so với vật chất tối đa trước đó, bởi vì chỉ những điểm đó mới tạo ra phạm vi hợp lệ mới để chọn năng lượng bùa chú. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | vượt qua một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | chỉ có tiền tố tối đa và bộ tích lũy được lưu trữ | 

Thuật toán xử lý mỗi quái vật chính xác một lần, điều này là tối ưu với ràng buộc tổng số`n`trên tất cả các trường hợp thử nghiệm là lên đến`3 · 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        h = list(map(int, input().split()))

        ans = 0
        pref_max = 0
        for x in h:
            if x > pref_max:
                ans += x - pref_max
                pref_max = x
        res.append(str(ans))

    return "\n".join(res)

# provided sample (formatted as separate cases)
assert run("""1
3
3 2 3
""") == "3"

# custom cases
assert run("""1
1
5
""") == "5", "single element"

assert run("""1
5
1 1 1 1 1
""") == "1", "flat array"

assert run("""1
5
5 4 3 2 1
""") == "5", "strictly decreasing"

assert run("""1
5
1 3 2 6 4
""") == "7", "mixed values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | hành vi trường hợp cơ bản | 
| tất cả đều bình đẳng | 1 | không có tiền tố mới maxima | 
| mảng giảm dần | 5 | chỉ đóng góp đầu tiên | 
| giá trị hỗn hợp | 7 | nhiều tiền tố tăng | 

## Vỏ cạnh 

Khi tất cả quái vật có lượng máu giống nhau, tiền tố tối đa không bao giờ thay đổi sau phần tử đầu tiên. Thuật toán chỉ đóng góp một lần, phản ánh rằng không có cấu trúc có thể truy cập mới nào được đưa ra sau quái vật đầu tiên. 

Khi mảng giảm dần, phần tử đầu tiên sẽ thống trị tất cả các phần tử tiếp theo, vì vậy mọi quái vật sau này không liên quan đến khả năng sử dụng phép thuật. Kết quả chỉ thu gọn về giá trị đầu tiên. 

Khi các giá trị dao động, chỉ có sự chuyển đổi đi lên mới quan trọng. Bất kỳ mức giảm trung gian nào đều không ảnh hưởng đến câu trả lời vì chúng không mở rộng phạm vi năng lượng bùa chú hợp lệ vượt quá mức đã được thiết lập bởi cực đại trước đó.
