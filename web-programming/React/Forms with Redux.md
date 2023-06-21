```
// constants.js
export const PARTICIPANT_REGISTER_FORM_SET_VALUE = 'PARTICIPANT_REGISTER_FORM_SET_VALUE';

export const PARTICIPANT_REGISTER_FORM_SUBMIT = 'PARTICIPANT_REGISTER_FORM_SUBMIT';
export const PARTICIPANT_REGISTER_FORM_SUBMIT_SUCCESS = 'PARTICIPANT_REGISTER_FORM_SUBMIT_SUCCESS';
export const PARTICIPANT_REGISTER_FORM_SUBMIT_FAILED = 'PARTICIPANT_REGISTER_FORM_SUBMIT_FAILED'; 
```

```
// reducer.js
import { 
    PARTICIPANT_REGISTER_FORM_SET_VALUE,
    PARTICIPANT_REGISTER_FORM_SUBMIT,
    PARTICIPANT_REGISTER_FORM_SUBMIT_SUCCESS,
    PARTICIPANT_REGISTER_FORM_SUBMIT_FAILED
 } from './constants';

const initialState = {
    form: {
        name: '',
        surname: '',
        numberOfPets: null,
        extraSocket: false,
        ownRack: false,
    }
    // Добавили новые поля в хранилище
    registrationRequest: false,
    registrationFailed: false,
}

export const participantRegistrationReducer = (state = initialState, action) => {
    switch(action.type) {
        case PARTICIPANT_REGISTER_FORM_SET_VALUE: {
            return {
                form: {
                    ...state.form,
                    [action.field]: action.value
                }
            }
        }
        case PARTICIPANT_REGISTER_FORM_SUBMIT: {
            return {
                ...state,
                registrationRequest: true,
                registrationFailed: false
            }
        }
        case PARTICIPANT_REGISTER_FORM_SUBMIT_SUCCESS: {
            return {
                ...state,
                form: {
                    // При успешной регистрацией сбрасываем форму до исходного состояния
                    ...initialState.form
                }
                registrationRequest: false
            }
        }
        case PARTICIPANT_REGISTER_FORM_SUBMIT_FAILED: {
            return {
                ...state,
                registrationRequest: false,
                registrationFailed: true
            }
        }
        default: {
            return state;
        }
    }
} 
```

```
// actions.js
import { 
    PARTICIPANT_REGISTER_FORM_SET_VALUE,
    PARTICIPANT_REGISTER_FORM_SUBMIT,
    PARTICIPANT_REGISTER_FORM_SUBMIT_SUCCESS,
    PARTICIPANT_REGISTER_FORM_SUBMIT_FAILED
 } from './constants';

export const setParticipantFormValue = (field, value) => ({
    type: PARTICIPANT_REGISTER_FORM_SET_VALUE,
    field,
    value
});

export const register = () => (dispatch, getState) => {
    dispatch({
    type: PARTICIPANT_REGISTER_FORM_SUBMIT
  });
    fetch('/api/register', {
        method: 'POST',
        // В тело запроса помещаем значения из хранилища
        body: JSON.stringify(...getState().participantRegistration.form)
    }).then(res=>{
        return res.json();
    }).then(data => {
        dispatch({
        type: PARTICIPANT_REGISTER_FORM_SUBMIT_SUCCESS,
            data
      });
    }).catch(err => {
        dispatch({
        type: PARTICIPANT_REGISTER_FORM_SUBMIT_FAILED,
      });
    })
} 
```

```
import React from 'react';
import { useSelector, useReducer } from 'react-redux';
import { setParticipantFormValue, register } from './services/register/actions'; 

export const Registration = () => {
    const {
        name,
        surname,
        numberOfPets,
        extraSocket,
        ownRack
    } = useSelector(state => state.participantRegistration.form);

    // Используем статус выполнения запроса для блокировки кнопки регистрации
    const { registrationRequest } = useSelector(state => state.participantRegistration);

    const dispatch = useDispatch();

    const onFormChange = (e) => {
        if (e.target.name === 'extraSocket' || e.target.name === 'ownRack') {
            dispatch(setParticipantFormValue(e.target.name, e.target.checked))
        } else {
            dispatch(setParticipantFormValue(e.target.name, e.target.value))
        }
    }

    const onFormSubmit = (e) => {
        // Предотвращаем дефолтное поведение формы при её отправке
        e.preventDefault();
        // Вызываем наш thunk-экшен
        dispatch(register())
    }

    return (
        <form onSubmit={onFormSubmit}>
            <label htmlFor="name">Имя</label>
            <input type="text" onChange={onFormChange} value={name} name="name" id="name" />

            <label htmlFor="surname">Фамилия</label>
            <input type="text" onChange={onFormChange} value={surname} name="surname" id="surname" />

            <label htmlFor="numberOfPets">Количество питомцев</label>
            <input type="number" min="1" onChange={onFormChange} value={numberOfPets} name="numberOfPets" id="numberOfPets"/>

            <label htmlFor="extraSocket">Требуется дополнительная розетка</label>
            <input type="checkbox" onChange={onFormChange} value={extraSocket} name="extraSocket" id="extraSocket"/>

            <label htmlFor="ownRack">Собственный стеллаж</label>
            <input type="checkbox" onChange={onFormChange} value={ownRack} name="ownRack" id="ownRack"/>
            
            <button type="submit" disabled={registrationRequest}>Зарегистрироваться</button>
        </form>
    )
} 
```

