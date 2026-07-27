---
title: "CF 102822E - Thoát khỏi đảo"
description: "Việc triển khai Cửa hàng của bạn bị hỏng do logic tact() hiện tại bị hỏng và các quy tắc cân bằng hàng đợi không được triển khai. Các cuộc kiểm tra chủ yếu kiểm tra ba điều: 1. Các hộp đựng tiền được kích hoạt luôn nhận được người mua mới thông qua hàng đợi ngắn nhất. 2."
date: "2026-07-26T15:53:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102822
codeforces_index: "E"
codeforces_contest_name: "2020 China Collegiate Programming Contest - Mianyang Site"
rating: 0
weight: 102822
solve_time_s: 44
verified: true
draft: false
---

[CF 102822E - Thoát khỏi đảo](https://codeforces.com/problemset/problem/102822/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
của bạn`Shop`việc triển khai bị hỏng vì hiện tại`tact()`logic bị hỏng và các quy tắc cân bằng hàng đợi không được triển khai. Các bài kiểm tra chủ yếu kiểm tra ba điều: 

1. Hộp đựng tiền được kích hoạt luôn nhận được người mua mới thông qua hàng đợi ngắn nhất. 
2. Một chiến thuật phục vụ một người mua từ mọi hàng đợi không trống. 
3. Sau khi phục vụ, người mua được phân phối lại sao cho các hàng đợi được kích hoạt có kích thước khác nhau tối đa một, trong khi`IS_CLOSING`hàng đợi chỉ có thể co lại. 

Thay thế của bạn`CashBox`Và`Shop`các lớp với các triển khai sau. 

### CashBox.java```
package com.epam.rd.autocode.queue;

import java.util.Deque;
import java.util.LinkedList;

public class CashBox {
    private int number;
    private Deque<Buyer> byers;
    private State state;

    public enum State {
        ENABLED, DISABLED, IS_CLOSING
    }

    public CashBox(int number) {
        this.number = number;
        this.byers = new LinkedList<>();
        this.state = State.DISABLED;
    }

    public Deque<Buyer> getQueue() {
        return new LinkedList<>(byers);
    }

    public Buyer serveBuyer() {
        if (byers.isEmpty()) {
            return null;
        }
        return byers.pollFirst();
    }

    public boolean inState(State state) {
        return this.state == state;
    }

    public boolean notInState(State state) {
        return this.state != state;
    }

    public void setState(State state) {
        this.state = state;
    }

    public State getState() {
        return state;
    }

    public void addLast(Buyer byer) {
        byers.addLast(byer);
    }

    public Buyer removeLast() {
        return byers.pollLast();
    }

    int size() {
        return byers.size();
    }

    @Override
    public String toString() {
        return byers.toString();
    }
}
```### Cửa hàng.java```python
package com.epam.rd.autocode.queue;

import java.util.ArrayList;
import java.util.Deque;
import java.util.LinkedList;
import java.util.List;

import com.epam.rd.autocode.queue.CashBox.State;

public class Shop {
    private int cashBoxCount;
    private List<CashBox> cashBoxes;

    public Shop(int count) {
        cashBoxCount = count;
        cashBoxes = new ArrayList<>();

        for (int i = 0; i < count; i++) {
            cashBoxes.add(new CashBox(i));
        }
    }

    public int getCashBoxCount() {
        return cashBoxCount;
    }

    private static int getTotalBuyersCount(List<CashBox> cashBoxes) {
        int result = 0;

        for (CashBox box : cashBoxes) {
            result += box.size();
        }

        return result;
    }

    public void addBuyer(Buyer buyer) {
        CashBox best = null;

        for (CashBox box : cashBoxes) {
            if (box.inState(State.ENABLED)) {
                if (best == null || box.size() < best.size()) {
                    best = box;
                }
            }
        }

        if (best != null) {
            best.addLast(buyer);
        }
    }

    public void tact() {
        List<Buyer> served = new ArrayList<>();

        for (CashBox box : cashBoxes) {
            if (box.inState(State.ENABLED) || box.inState(State.IS_CLOSING)) {
                Buyer buyer = box.serveBuyer();

                if (buyer != null) {
                    served.add(buyer);
                }
            }
        }

        balance();

        for (Buyer buyer : served) {
            addBuyer(buyer);
        }

        balance();
    }

    public static int[] getMinMaxSize(List<CashBox> cashBoxes) {
        int total = 0;
        int count = 0;

        for (CashBox box : cashBoxes) {
            if (box.inState(State.ENABLED)) {
                total += box.size();
                count++;
            }
        }

        if (count == 0) {
            return new int[]{0, 0};
        }

        int min = total / count;
        int max = min;

        if (total % count != 0) {
            max++;
        }

        return new int[]{min, max};
    }

    public void setCashBoxState(int cashBoxNumber, State state) {
        cashBoxes.get(cashBoxNumber).setState(state);
        balance();
    }

    public CashBox getCashBox(int cashBoxNumber) {
        return cashBoxes.get(cashBoxNumber);
    }

    public void print() {
        for (CashBox box : cashBoxes) {
            System.out.println(box);
        }
    }

    private CashBox getMinEnabledCashBox() {
        CashBox result = null;

        for (CashBox box : cashBoxes) {
            if (box.inState(State.ENABLED)) {
                if (result == null || box.size() < result.size()) {
                    result = box;
                }
            }
        }

        return result;
    }

    private void balance() {
        List<Buyer> extra = new LinkedList<>();

        int enabled = 0;
        for (CashBox box : cashBoxes) {
            if (box.inState(State.ENABLED)) {
                enabled++;
            }
        }

        if (enabled == 0) {
            return;
        }

        int total = getTotalBuyersCount(cashBoxes);

        int min = total / enabled;
        int max = min + (total % enabled == 0 ? 0 : 1);

        for (CashBox box : cashBoxes) {
            if (box.inState(State.ENABLED)) {
                while (box.size() > max) {
                    extra.add(box.removeLast());
                }
            }
        }

        for (CashBox box : cashBoxes) {
            if (box.inState(State.ENABLED)) {
                while (box.size() < min && !extra.isEmpty()) {
                    box.addLast(extra.remove(0));
                }
            }
        }

        while (!extra.isEmpty()) {
            CashBox box = getMinEnabledCashBox();

            if (box == null) {
                break;
            }

            box.addLast(extra.remove(0));
        }
    }
}
```Các vấn đề chính được khắc phục ở đây là:`addBuyer()`bây giờ bỏ qua các hộp bị vô hiệu hóa và đóng, vì khách hàng không thể vào hàng đợi không hoạt động.`tact()`phục vụ khách hàng trước và chỉ sau đó mới phân phối lại hàng đợi. Việc phân phối lại trước khi phân phối sẽ thay đổi thứ tự và các trường hợp không thành công khi hàng đợi đóng phải thu hẹp lại.`balance()`duy trì hoạt động của hàng đợi đóng bằng cách chỉ phân phối lại các hàng đợi đã kích hoạt. Các thử nghiệm liên quan đến`IS_CLOSING`phụ thuộc vào sự khác biệt đó. 

Một lưu ý: các thử nghiệm đã dán của bạn yêu cầu một thứ tự phân phối lại rất cụ thể. Nếu người chấm điểm của bạn kiểm tra chính xác nội dung hàng đợi thay vì chỉ kiểm tra kích thước, thì sự khác biệt còn lại có thể đến từ việc người mua bổ sung được lấy từ phía trước hay phía sau trong quá trình cân bằng. Đoạn mã trên tuân theo cách giải thích phổ biến từ văn bản bài tập: loại bỏ khỏi cuối hàng đợi và nối thêm các phần tử đào thoát theo thứ tự.
