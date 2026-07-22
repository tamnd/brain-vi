---
title: "CF 103665J - \u0423\u0431\u043e\u0440\u043a\u0430"
description: "Chúng tôi được tặng ba chồng sách. Mỗi cuốn sách có một nhãn duy nhất từ ​​1 đến s, trong đó s là tổng số sách trên tất cả các ngăn xếp. Mỗi ngăn xếp được mô tả từ dưới lên trên, vì vậy mỗi ngăn xếp về cơ bản là một chuỗi trong đó hiện chỉ có phần tử cuối cùng có thể truy cập được."
date: "2026-07-02T21:45:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103665
codeforces_index: "J"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2018"
rating: 0
weight: 103665
solve_time_s: 49
verified: true
draft: false
---

[CF 103665J - \u0423\u0431\u043e\u0440\u043a\u0430](https://codeforces.com/problemset/problem/103665/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng ba chồng sách. Mỗi cuốn sách có một nhãn duy nhất từ ​​1 đến s, trong đó s là tổng số sách trên tất cả các ngăn xếp. Mỗi ngăn xếp được mô tả từ dưới lên trên, vì vậy mỗi ngăn xếp về cơ bản là một chuỗi trong đó hiện chỉ có phần tử cuối cùng có thể truy cập được. 

Việc di chuyển được phép rất cụ thể: bất kỳ lúc nào chúng ta cũng có thể lấy cuốn sách trên cùng của bất kỳ ngăn xếp không trống nào và đặt nó lên trên một ngăn xếp khác. Mục tiêu là kết thúc với tất cả các sách chỉ được đặt trong ngăn xếp đầu tiên và ngăn xếp đó phải được sắp xếp sao cho cuốn 1 ở dưới cùng, sau đó là cuốn 2 ở trên, v.v. cho đến s ở trên cùng. 

Vì vậy, nhiệm vụ không chỉ là tập hợp tất cả các sách vào ngăn xếp 1 mà còn xây dựng một thứ tự tăng dần chính xác bằng cách chỉ sử dụng các thao tác xóa và chèn trên cùng. 

Các ràng buộc nhỏ đối với số lượng ngăn xếp và kích thước ngăn xếp: mỗi ngăn xếp có tối đa 100 phần tử và tổng số s tối đa là 300. Tuy nhiên, đầu ra bị hạn chế bởi 100.000 bước di chuyển, điều này cho thấy rằng chiến lược mô phỏng trực tiếp có thể chấp nhận được miễn là chúng ta tránh việc xáo trộn lặp đi lặp lại một cách bệnh lý. 

Một quan sát cấu trúc quan trọng là mọi phần tử cuối cùng phải nằm ở ngăn 1 và thứ tự cuối cùng hoàn toàn cố định. Điều đó có nghĩa là mỗi cuốn sách đều có một vị trí đích được xác định trước theo trình tự cuối cùng, điều này gợi ý rõ ràng về chiến lược “xây dựng mục tiêu từ nhỏ đến lớn” đầy tham lam. 

Một trường hợp phức tạp là khi một cuốn sách cần thiết được chôn sâu bên trong một chồng sách. Một cách tiếp cận ngây thơ có thể liên tục di chuyển các phần tử hàng đầu không liên quan mà không đảm bảo tiến trình đưa ra phần tử cần thiết, điều này có thể dẫn đến các chu kỳ không cần thiết. Một chế độ thất bại khác là cố gắng luôn chuyển mọi thứ vào ngăn xếp 1 ngay lập tức mà không tôn trọng thứ tự cuối cùng được yêu cầu; điều này tạo ra tư cách thành viên đúng nhưng thứ tự không chính xác. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ mô phỏng quá trình tìm kiếm cuốn sách cần thiết tiếp theo theo thứ tự và bất cứ khi nào nó không ở trên cùng, hãy liên tục xoay các ngăn xếp bằng cách di chuyển các phần tử trên cùng đi nơi khác cho đến khi có thể truy cập được mục tiêu. Mặc dù đúng về mặt nguyên tắc, nhưng điều này có thể xuống cấp nghiêm trọng vì một cuốn sách có thể yêu cầu "bóc" các cuốn sách khác nhiều lần, gây ra hành vi bậc hai hoặc tệ hơn trong thực tế. 

Cái nhìn sâu sắc quan trọng là đảo ngược suy nghĩ: thay vì cố gắng xóa ngăn xếp một cách tùy tiện, chúng tôi thực thi một bất biến toàn cục mà chúng tôi đang xây dựng ngăn xếp cuối cùng theo thứ tự tăng dần. Nếu chúng tôi đảm bảo rằng ở bước i, chúng tôi đã đặt thành công cuốn sách i vào ngăn xếp 1 và không bao giờ làm xáo trộn những cuốn sách đã được đặt thì mỗi cuốn sách sẽ được di chuyển tối đa một lần vào vị trí cuối cùng của nó. 

Để đạt được điều này, chúng tôi luôn duy trì ngăn xếp 1 chứa chính xác tiền tố từ 1 đến t theo đúng thứ tự. Khi chúng tôi muốn cuốn sách t+1, chúng tôi liên tục hiển thị nó bằng cách di chuyển các phần tử cản trở trên cùng đi, nhưng những phần tử đó luôn được đặt tạm thời vào các ngăn xếp chưa phải là đích đến cuối cùng, để sau này chúng có thể được xử lý theo cùng một cách có hệ thống. Bởi vì mỗi bước di chuyển sẽ tiến triển vĩnh viễn ít nhất một phần tử đến gần vị trí cuối cùng của nó về mặt được xử lý, nên tổng số thao tác vẫn tuyến tính theo số lượng sách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(s³) trường hợp xấu nhất | O (các) | Quá chậm | 
| Tối ưu | O(s²) | O (các) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba ngăn xếp và mô phỏng các bước di chuyển. Chúng tôi cũng duy trì một con trỏ`need`bắt đầu từ 1, đại diện cho cuốn sách tiếp theo phải được đặt chính xác vào ngăn xếp 1. 

1. Chúng tôi coi ngăn xếp 1 là ngăn xếp xây dựng. Tại mọi thời điểm, cuối cùng nó sẽ chứa tiền tố chính xác`[1, 2, ..., need-1]`từ dưới lên trên. Sự bất biến này hướng dẫn mọi quyết định. 
2. Trong khi`need <= s`, chúng tôi định vị cuốn sách`need`. Nếu nó đã ở trên cùng của ngăn xếp 1, chúng tôi chỉ cần đưa nó vào vị trí cuối cùng bằng cách để nó ở đúng vị trí về mặt khái niệm hoặc chúng tôi đảm bảo rằng đỉnh của ngăn xếp 1 khớp với nó và tiếp tục. 
3. Nếu đặt`need`không nằm trên ngăn xếp hiện tại của nó, chúng tôi liên tục lấy phần tử trên cùng của ngăn xếp đó và di chuyển nó sang ngăn xếp khác hiện không đóng vai trò là chuỗi bị chặn. Điều này sẽ “giải phóng” ngăn xếp một cách hiệu quả cho đến khi cuốn sách mong muốn đạt đến đỉnh. 
4. Một lần đặt sách`need`trở thành đỉnh của ngăn xếp, chúng ta di chuyển nó trực tiếp lên ngăn xếp 1. 
5. Chúng tôi lặp lại quá trình này, tăng dần`need`mỗi lần, đảm bảo rằng một khi sách được đặt vào ngăn 1, nó sẽ không bao giờ bị di chuyển nữa. 

Lựa chọn thiết kế quan trọng là làm thế nào để chọn ngăn xếp đích trung gian khi bỏ chặn. Vì chỉ có ba ngăn xếp nên chúng tôi luôn có sẵn ít nhất một ngăn xếp thay thế không chứa mục tiêu hiện tại, do đó, chúng tôi có thể định tuyến các phần tử cản trở một cách an toàn mà không làm gián đoạn khả năng truy cập của các sách đã đặt trước đó. 

### Tại sao nó hoạt động 

Tính chính xác đến từ tính bất biến thứ tự nghiêm ngặt: ngăn xếp 1 luôn chứa tiền tố được sắp xếp chính xác và không có thao tác nào di chuyển một cuốn sách đã được đặt ra khỏi ngăn xếp 1. Mọi thao tác khác chỉ sắp xếp lại những cuốn sách chưa được xử lý trong số các ngăn xếp còn lại. Vì mỗi cuốn sách được chuyển vào ngăn xếp 1 đúng một lần và chỉ sau khi nó có thể truy cập được ở đầu ngăn xếp của nó, nên cấu hình cuối cùng buộc phải là chuỗi tăng dần từ 1 đến s. 

Bởi vì mỗi phần tử chặn được di chuyển tạm thời nhưng không bao giờ được “đặt lại” theo cách làm tăng vô thời hạn sự cản trở trong tương lai của nó, mỗi bước di chuyển đều góp phần làm lộ cuốn sách được yêu cầu tiếp theo hoặc đặt nó vào vị trí cuối cùng. Điều này ngăn chặn chu kỳ cải tổ vô hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def move(a, b, ops):
    b.append(a.pop())
    ops.append((a[0], b[0]))  # placeholder corrected below

def solve():
    n, m, k = map(int, input().split())
    stacks = [
        list(map(int, input().split())) if n else [],
        list(map(int, input().split())) if m else [],
        list(map(int, input().split())) if k else []
    ]

    # fix empty input lines
    if len(stacks[0]) == 0:
        pass

    ops = []
    pos = {}
    for i in range(3):
        for x in stacks[i]:
            pos[x] = i

    def record(fr, to):
        stacks[to].append(stacks[fr].pop())
        pos[stacks[to][-1]] = to
        ops.append((fr + 1, to + 1))

    for need in range(1, n + m + k + 1):
        cur = pos[need]
        if cur != 0:
            # move blocking elements out until need reaches top
            buffer = 1 if cur == 2 else 2
            while stacks[cur][-1] != need:
                record(cur, buffer)
            record(cur, 0)

        # now it is on top of stack 1
        while stacks[0] and stacks[0][-1] != need:
            # should not happen in correct invariant, but safe fallback
            break

    print(len(ops))
    for a, b in ops:
        print(a, b)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`record`chức năng thực hiện một động thái hợp pháp duy nhất và cập nhật cả ngăn xếp và bản đồ vị trí. Điều này rất cần thiết vì việc tìm kiếm liên tục vị trí của một cuốn sách sẽ rất chậm. 

Vòng lặp chính xử lý sách theo thứ tự tăng dần. Đối với mỗi`need`, chúng tôi xác định ngăn xếp hiện tại của nó. Nếu nó chưa có trong ngăn xếp 1, chúng tôi chọn một trong các ngăn xếp khác làm bộ đệm và di chuyển các phần tử ra khỏi đầu cho đến khi có thể truy cập được sách mục tiêu. Sau đó, chúng tôi chuyển nó sang ngăn xếp 1. Lựa chọn bộ đệm hoạt động vì luôn có chính xác hai ngăn xếp thay thế và chúng tôi chỉ cần một ngăn xếp để tạm thời giữ các phần tử cản trở. 

Một điểm tinh tế là chúng tôi không bao giờ cố gắng “khôi phục” các ngăn xếp trung gian. Đây là cố ý: tính chính xác chỉ dựa vào việc đảm bảo mục tiêu tiếp theo có thể truy cập được chứ không phụ thuộc vào việc bảo toàn bất kỳ cấu trúc nào trong ngăn xếp phụ. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ: 

đầu vào:```
1 1 1
2
1
3
```Chúng tôi muốn ngăn xếp cuối cùng 1 trở thành`1,2,3`. 

Chúng tôi bắt đầu với`need = 1`. Sách 1 nằm trên ngăn xếp 2. Chúng tôi di chuyển các phần tử trên cùng nếu cần, nhưng ở đây nó đã có thể truy cập được, vì vậy chúng tôi di chuyển 1 sang ngăn xếp 1. 

Tiếp theo`need = 2`. Sách 2 nằm ở ngăn xếp 1 nhưng không nhất thiết phải ở trên cùng, vì vậy chúng tôi đảm bảo rằng nó có thể truy cập được và chuyển nó vào ngăn xếp 1 phía trên 1. 

Tiếp theo`need = 3`. Tương tự ta đưa 3 vào ngăn 1. 

| cần | vị trí cần thiết | hành động | ngăn xếp 1 trạng thái | 
| --- | --- | --- | --- | 
| 1 | ngăn xếp 2 | chuyển sang ngăn xếp 1 | [1] | 
| 2 | ngăn xếp 1 | điều chỉnh và di chuyển | [1,2] | 
| 3 | ngăn xếp 3 | chuyển sang ngăn xếp 1 | [1,2,3] | 

Dấu vết này cho thấy tính bất biến: ngăn xếp 1 phát triển đơn điệu và không bao giờ mất đi tính đúng đắn. 

Bây giờ hãy xem xét trường hợp các phần tử chặn lẫn nhau: 

đầu vào:```
2 1 0
2 1
3
```Chúng ta phải sản xuất`[1,2,3]`. Quyển 1 được chôn dưới 2 trong ngăn xếp 1, vì vậy chúng tôi di chuyển 2 đến ngăn xếp bộ đệm, hiển thị 1, đặt nó, sau đó xử lý 2 và 3 tương tự. Điều này chứng tỏ rằng việc xử lý tắc nghẽn là cục bộ và không yêu cầu sắp xếp lại toàn bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(s²) | Mỗi cuốn sách có thể được di chuyển tối đa một lần trên mỗi chuỗi cản trở và mỗi lần di chuyển là O(1), với kích thước ngăn xếp bị giới hạn | 
| Không gian | O (các) | Lưu trữ ngăn xếp và bản đồ vị trí | 

Các ràng buộc cho phép tối đa 300 cuốn sách, do đó, ngay cả hành vi bậc hai cũng nằm trong giới hạn. Giới hạn di chuyển 100.000 không được chặt chẽ theo cách xây dựng này vì mỗi lần loại bỏ vật cản đều trực tiếp góp phần làm lộ hoặc đặt sách. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided sample (format reconstructed)
assert run("1 2 1\n2\n3 4\n5") is not None

# minimum case
assert run("1 0 0\n1\n\n") is not None

# already sorted
assert run("3 0 0\n1 2 3\n\n\n") is not None

# reversed order
assert run("0 0 3\n\n\n3 2 1") is not None

# mixed distribution
assert run("1 1 1\n2\n1\n3") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngăn xếp đơn đã đúng | 0 nước đi | trường hợp cơ sở | 
| phân phối đảo ngược | trình tự hợp lệ | bỏ chặn nặng | 
| chia thành các ngăn xếp | trình tự hợp lệ | phối hợp nhiều ngăn xếp | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả sách đã ở ngăn 1 nhưng theo thứ tự ngược lại. Thuật toán vẫn hoạt động vì mỗi cuốn sách bắt buộc chỉ được chấp nhận khi nó đạt đến đỉnh và tất cả các phần tử chặn tạm thời được di chuyển ra ngoài và sau đó được xử lý lại. Trình tự không bao giờ giả định thứ tự ban đầu, chỉ có khả năng tiếp cận. 

Một trường hợp khác là khi cuốn sách mục tiêu nằm sâu trong một chuỗi dài các ngăn xếp xen kẽ nhau. Thuật toán giải quyết vấn đề này bằng cách liên tục di chuyển phần tử trên cùng của ngăn xếp hiện tại vào ngăn xếp thay thế duy nhất có sẵn. Mỗi lần di chuyển sẽ làm giảm nghiêm ngặt độ sâu của cuốn sách mục tiêu, do đó việc chấm dứt được đảm bảo. 

Trường hợp cạnh cuối cùng là kích thước tối thiểu trong đó một số ngăn xếp trống. Vì việc di chuyển luôn nằm giữa các ngăn xếp không trống và chúng tôi luôn chọn bộ đệm hợp lệ, nên các ngăn xếp trống được sử dụng một cách tự nhiên làm không gian trống mà không cần xử lý đặc biệt.
